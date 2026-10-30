# Working with the voice files

## Names

Every file carries the person's full name, so voices shared between people never get mixed up. For Jane Smith:

- Folder: **Jane Smith Voice**
- Rules doc: **Jane Smith Voice Rules** (Google Doc)
- Log: **Jane Smith Voice Corrections Log** (Google Sheet)
- Inside the folder: **Old versions**, holding replaced copies named "Jane Smith Voice Rules (replaced YYYY-MM-DD)"

In these instructions, [Name] means the person's full name exactly as it appears in their folder name. This needs the Google Drive and Google Sheets connectors.

Drive's search is loose: even a `title =` query returns near matches, such as an old "Voice_Profile" file. Always filter by file type in the query, and check the exact title of every result yourself before using it.

## Finding the user's own files

1. **Rules doc:** `search_files` with `title contains 'Voice Rules' and mimeType = 'application/vnd.google-apps.document' and owner = 'me'`. Keep only a result titled exactly "[Name] Voice Rules", with no brackets after it. Its `parentId` is the user's Voice folder. If there's more than one, use the one whose folder is named "[Name] Voice" and tell the user in one line that there's a duplicate.
2. **Log:** `search_files` with `parentId = '<folder id>' and mimeType = 'application/vnd.google-apps.spreadsheet'`, then take the result titled exactly "[Name] Voice Corrections Log".
3. **Old versions:** `search_files` with `parentId = '<folder id>' and mimeType = 'application/vnd.google-apps.folder'`, then take the result titled exactly "Old versions".

Always include `owner = 'me'` in step 1, so a voice someone else has shared with the user is never mistaken for theirs.

## Someone else's voice

When the user writes as someone who has shared their Voice folder with them:

1. `search_files` with `title contains 'Voice Rules' and mimeType = 'application/vnd.google-apps.document'`. Follow the next-page token until none comes back. Keep only results titled exactly "[Someone's name] Voice Rules", with no brackets after it, and pick the one whose name matches the person the user means. If two match (two Sarahs, say), ask the user which one.
2. If nothing matches, that person hasn't shared their folder with the user. Say so in one line and fall back to the "without a shared Rules doc" case in the skill.
3. Their log: `search_files` with `parentId = '<their Rules doc's parentId>' and mimeType = 'application/vnd.google-apps.spreadsheet'`, then take the result titled exactly "[Their name] Voice Corrections Log". If the user can't see it, they can't log to it.
4. Read their Rules doc exactly as you would the user's own (see Reading).

Don't use `owner != 'me'` in any query: Drive then ignores the title match and returns unrelated files.

## Reading

- **Rules doc:** `read_file_content` with `includeComments: true`. Without it, Drive's reader garbles emoji and cuts the end off long documents. Headings come back wrapped in `**` bold markers. Ignore the markers.
  - This reader cuts a document off at about 32,000 characters. That's why the Rules doc has a 27,000-character limit and always ends with the "Default rules switched off" heading. If that heading is missing from what you read, the read was cut short. Never rebuild the doc from a cut-short read.
  - This reader also garbles emoji (they come back as stray symbols such as "ð"), so the Rules doc never contains emoji characters. Describe them in words instead, for example "Let me know down there, then the pointing-down emoji".
- **Log:** Google Sheets `get_values` with range `A1:L3000`. Row 1 is the header row. Spreadsheet row number = position in the returned list + 1.

Leave the tab name out of every range. Ranges without a tab name go to the first tab, and the tab name varies between accounts.

## Writing to the log

- **Add a row:** `append_values` with range `A1` and one row of 12 values in column order. Work out the next ID from the last row you read.
- **Change a status:** `update_values` with range `J<row>:K<row>` and the values `[status, date]`. Before writing, read that row and check its ID, so you never change the wrong row.

## Writing the Rules doc

Only the voice-review and voice-setup skills write the Rules doc.

**Size.** Keep the whole doc under 27,000 characters, about 4,500 words. Count before creating it.

**Format.** Use headings (`#`, `##`, `###`), bullet points, plain paragraphs and **bold** only. Tables and quote blocks don't survive Drive's conversion: they come back mangled on the next read and get worse with every rebuild. No emoji characters (see Reading).

**Creating:** `create_file` with `contentMimeType` `text/markdown`, the Markdown as `textContent`, and `parentId` set to the folder. Drive turns it into a Google Doc.

**Replacing:** Drive can't overwrite a doc's text, so a new version replaces the old one. Work in this order, which never leaves the user without a working doc:

1. Create the new version with the title "[Name] Voice Rules (new)" in the Voice folder.
2. Read it back. Check that every `##` heading from the old version is still there, except headings the user approved removing. Check that the length is in line with the changes: for added rules, about the same or longer. If either check fails, trash the new doc, tell the user nothing changed, and stop.
3. Rename the old doc to "[Name] Voice Rules (replaced YYYY-MM-DD)" and move it into Old versions with `update_file` (`title`, `parentId`). If Old versions doesn't exist, create it first (`create_file`, mimeType `application/vnd.google-apps.folder`).
4. Rename the new doc to "[Name] Voice Rules".

## Creating the log

`create_file` with `contentMimeType` `text/csv`, `parentId` set to the folder, and this header line as `textContent`:

```
ID,Date,Register,Context,What Claude wrote,What I changed it to,My words,The rule it suggests,Confidence,Status,Status date,Notes
```

Drive turns it into a Google Sheet.
