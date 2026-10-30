# Working with the My Voice files

All three files are in the user's Google Drive, in the folder **My Voice**. This needs the Google Drive and Google Sheets connectors.

## Finding them

The user's own files, all with `owner = 'me'` so a voice someone has shared with them is never mistaken for theirs:

1. **Folder:** `search_files` with `title = 'My Voice' and mimeType = 'application/vnd.google-apps.folder' and owner = 'me'`. If there's more than one, use the one that contains "My Voice Rules".
2. **Rules doc:** `search_files` with `title = 'My Voice Rules' and parentId = '<folder id>'`.
3. **Log:** `search_files` with `title = 'My Voice Corrections Log' and parentId = '<folder id>'`.
4. **Old versions:** `search_files` with `title = 'Old versions' and parentId = '<folder id>'`.

Match the titles exactly. Replaced copies are named "My Voice Rules (replaced YYYY-MM-DD)", so they never match the live one. If two files have the exact live title, use the most recently modified one and tell the user in one line that there's a duplicate.

## Someone else's voice

When the user writes as someone who has shared their My Voice folder with them:

1. `search_files` with `title = 'My Voice Rules'`. Keep only results whose title is exactly "My Voice Rules", then pick the one whose `owner` is that person. Don't use `owner != 'me'` in the query: Drive ignores the title match when you do and returns unrelated files.
2. If none of the results belongs to that person, they haven't shared their folder with the user. Say so in one line and fall back to the "without a shared Rules doc" case in the skill.
3. Their log: `search_files` with `title = 'My Voice Corrections Log'`, again picking the exact title with their `owner`.
4. Read their Rules doc exactly as you would the user's own (see Reading).

## Reading

- **Rules doc:** `read_file_content`. Headings come back wrapped in `**` bold markers. Ignore the markers.
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

1. Create the new version with the title "My Voice Rules (new)" in the My Voice folder.
2. Read it back. Check that every `##` heading from the old version is still there, except headings the user approved removing. Check that the length is in line with the changes: for added rules, about the same or longer. If either check fails, trash the new doc, tell the user nothing changed, and stop.
3. Rename the old doc to "My Voice Rules (replaced YYYY-MM-DD)" and move it into Old versions with `update_file` (`title`, `parentId`). If Old versions doesn't exist, create it first (`create_file`, mimeType `application/vnd.google-apps.folder`).
4. Rename the new doc to "My Voice Rules".

## Creating the log

`create_file` with `contentMimeType` `text/csv`, `parentId` set to the folder, and this header line as `textContent`:

```
ID,Date,Register,Context,What Claude wrote,What I changed it to,My words,The rule it suggests,Confidence,Status,Status date,Notes
```

Drive turns it into a Google Sheet.
