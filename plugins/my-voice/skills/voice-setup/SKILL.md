---
name: voice-setup
description: Sets up the My Voice system for a new user. Finds their existing voice profile, builds their Voice Rules Google Doc and corrections log, gives them the line to add to their Claude settings, tests the correction loop and schedules the weekly review. Use when the user says set up my voice, voice setup, set up the voice system, or start my voice rules again.
---

# Voice setup

You're setting up the My Voice system for this user. At the end they'll have:

- a Voice folder in their Google Drive, named after them, holding their Rules doc, their corrections log and an Old versions folder
- a short block in their Claude settings
- a weekly scheduled review

Aim for 20 minutes.

Before starting, read these three files in this plugin: `skills/my-voice/references/drive.md`, `skills/my-voice/references/rules-template.md` and `skills/my-voice/references/default-rules.md`.

## How to run it

- Read what you can before asking. Never make the user type what you can find.
- Ask in small batches, at most 3 questions at a time. Use multiple choice where there are clear options.
- Create nothing until step 6, apart from what the user asks for.

## 1. Check what's connected

Google Drive and Google Sheets are required. If either is missing, tell the user to connect it from Settings, then Connectors, then start a new chat and say "set up my voice" again. Then stop.

Check whether a scheduling tool is available in this chat. It's needed in step 9.

## 2. Check for an existing setup

Search for the user's own Voice Rules doc (see drive.md). If one exists with a Rules doc in it, ask whether to start again or stop. Starting again moves the current Rules doc into Old versions and keeps the log.

## 3. Find their voice material

Search their Drive for titles containing "Voice Profile", "Voice Notes", "About Me" or "Voice", and check this chat for an attached file. Show what you found and ask which to use. Read the chosen files in full.

If there's nothing, ask them to attach or paste their voice profile. If they don't have one, ask for 3 to 5 pieces they wrote themselves, ideally for the kinds of writing they ask Claude for most. Tell them plainly: writing that Claude or another AI drafted teaches the system its own habits, so use pieces they wrote with their own hands.

## 4. Ask what the material can't tell you

Batch these. Skip any the material already answers.

1. Their full name, exactly as it should appear on their files, and the name Claude should call them by.
2. Which spelling: UK or US English?
3. Which kinds of writing do they get Claude to do? Offer: chat with Claude, business email, LinkedIn posts and articles, LinkedIn messages, newsletters or marketing emails, website and sales pages, proposals, reports and client documents, spoken scripts. Let them pick several.
4. Does their email add a signature automatically? If yes, drafts never type a sign-off.
5. Does Claude ever write in someone else's name for them, such as a colleague or a client?
6. Any words or phrases they never want to see?
7. Does anyone else write in their name, such as an assistant or their LinkedCoach team? If yes, get each person's email address.
8. If the material says nothing about chat: when Claude talks to them, how do they want it? For example short and direct, or more detailed.

## 5. Build the Rules doc

Write the Rules doc in Markdown, following `rules-template.md`.

- **Only rules that change how something gets written.** Cut biography, values and flattering description. Test each line: if it disappeared, would Claude write differently? If not, cut it.
- **Mark each register honestly.** Established only where the material has real evidence for that register, meaning examples or specific rules. Thin where there's a little. Not established where there's nothing. A register with no evidence stays empty. Never fill it with rules borrowed from another register.
- **Keep their own words.** Quote phrases and examples verbatim from the material.
- **Don't copy the default rules in.** They're applied separately. If the material shows the user really does something a default rule bans (a word on the D6 list, say), add it under "Default rules switched off" with the reason, and confirm it with them.
- **Keep it under 4,000 words (about 24,000 characters)** to start, so there's room to grow under the 27,000-character limit. No emoji characters: describe them in words.

Show them a short summary: the registers with their status, and 5 of the most distinctive rules. Ask what's wrong or missing. Fix it.

## 6. Create the files

In their Drive (in My Drive unless they name a folder):

Use their full name, as they confirmed it in step 4, in every file name (see "Names" in drive.md).

1. Create the folder "[Name] Voice".
2. Create the Rules doc "[Name] Voice Rules" from the Markdown, in that folder.
3. Create the log "[Name] Voice Corrections Log" from the header line in drive.md, in that folder.
4. Create the folder "Old versions" inside it.

Read the Rules doc and the log back to check they're complete. Give them the folder link.

If anyone else writes in their name (question 7), share with each of them using `share_file`: the Voice folder as `reader`, and the log as `writer`. With that access, the other person's Claude writes from these rules whenever it writes as the user, and their edits go into the log for the user's weekly review. Tell the user they can remove anyone's access in Drive at any time.

## 7. A short block in their Claude settings

Explain in one sentence: the voice skill loads when Claude sees a writing task, and this block makes sure corrections made in ordinary conversation get caught too.

Ask them to paste this into their Claude settings, in the personal preferences box, below anything already there. Fill in their full name first:

```
# Voice and corrections
- Before writing anything, work out whose voice it is. If I'm writing for someone else, their voice wins and mine is switched off. If you can't tell, ask.
- For my own writing, use the my-voice skill. My rules are in "[Name] Voice Rules" in my Google Drive.
- Whenever I edit, rewrite or object to something you wrote for me, including in conversation, use my-voice to log the correction. Don't wait to be asked. Confirm in one line.
```

If their personal preferences already describe their writing style or point at an old voice profile file, suggest moving those rules into the Rules doc and deleting the pointer, so there's one place to keep up to date.

## 8. Test the loop

Offer a short real test. Write something small they need anyway, such as a reply to an email or a short post, using the new rules. Ask them to change one line the way they'd normally edit it. Log the correction with the my-voice skill and show them the new row. This proves the whole loop works before the first weekly review.

## 9. Schedule the weekly review

Ask which day and time suits a 10-minute weekly review. Suggest Monday morning.

If a scheduling tool is available, create a weekly scheduled task named "Weekly voice review" with exactly this instruction:

> Run my weekly voice review with the voice-review skill. My rules and corrections log are in the "[Name] Voice" folder in my Google Drive. Present the proposals and wait for my answers in this chat. Change nothing until I've approved it. If the voice-review skill isn't available in this run, write one line saying "Voice review didn't run: the voice-review skill wasn't available."

If there's no scheduling tool, give them that text and tell them to create a weekly task from Scheduled in the sidebar.

Tell them how the review works:

1. They'll get a notification when the review is ready. They open it and answer the proposals in that chat.
2. They can also say "review my voice" in any chat, at any time.
3. Every review ends with the line "My Voice review" and a version number. If that line is missing, the review didn't run properly.

## 10. Finish

Finish in three lines: the folder link, the day and time of the first review, and that from now on every correction they make gets logged.
