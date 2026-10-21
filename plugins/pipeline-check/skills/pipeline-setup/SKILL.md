---
name: pipeline-setup
description: Set up the daily pipeline check for a new user. Interviews them, reads their CRM and connected apps, writes their pipeline settings Google Doc, runs a test and creates the scheduled task. Use when the user says set up my pipeline check, pipeline setup, set up the pipeline check, or change my pipeline settings from scratch.
---

# Pipeline check setup

You're setting up the daily pipeline check (the `pipeline-check` skill) for this user. The result is one settings document in their Google Drive, a test run, and a scheduled task. Usually a LinkedCoach team member is on a call with the user while this runs, so keep it moving: aim for 30 minutes.

## How to run the interview

- Work out as much as you can from the user's CRM and apps first, then ask them to confirm or correct. Never make them type what you can read.
- Ask in small batches. Use multiple-choice questions where there are clear options; otherwise ask in plain text, at most three questions at a time.
- Use the user's own names for things (their pipeline names, stage names, quiz names).
- Write nothing to the CRM or any app during setup. The only thing you create is the settings document, and the scheduled task at the end.
- The settings template in the pipeline-check skill (`skills/pipeline-check/references/settings-template.md` in this plugin) shows every section of the settings document, with an example. The document you write must follow it section by section, using the same headings.

## 1. Check what's connected

Look at the tools available in this chat and list, in one short message, which of these are connected and which aren't:

- **Required:** the Less Annoying CRM connector (`crm_*` tools) and Google Drive.
- **Lead sources and channels:** email (Gmail or Outlook), calendar, booking tool (Calendly, OnceHub or similar), quiz tool (ScoreApp or similar), email marketing (Kit or similar), meeting notes (Fathom, Wispr Flow, Fireflies, Otter, Google Meet notes by email), LinkedIn inbox (Kondo), payment emails.

If the CRM connector is missing, stop and say: "Connect the CRM first: Customize, Plugins, Pipeline Check, Connectors, then connect less-annoying-crm with your Less Annoying CRM API key. Then start a new chat and run setup again." If Google Drive is missing, ask them to connect it and start again.

Ask which apps they use for leads that aren't connected yet. Let them connect those now. If the new app's tools don't appear in this chat, tell them to start a new chat and say "set up my pipeline check" again. Anything they don't use is switched off in the settings.

## 2. About them

Fill from what you can see, then confirm: name, business in one line, time zone, report language (UK or US English), and their own email address. Explain that +aliases of that address (name+test@...) count as test entries, and the plain address never does.

## 3. Pipelines

1. Call `crm_list_pipelines`. Show each pipeline with its stages.
2. Ask which pipelines are sales pipelines. Client delivery pipelines are left out entirely: the check never touches existing clients' delivery work.
3. For each sales pipeline, mark which stages are won, lost or not qualified (propose from the names, they confirm).
4. **Check each sales pipeline has the stages the check relies on, before going further:** a not-qualified stage (for leads from discarded locations), and a stage for after a call has happened (usually "Follow Up"). The connector can't add stages, so if one is missing, tell them how: in Less Annoying CRM, Settings, then Pipelines, open the pipeline, add the stage, and mark not-qualified as a closed stage. Let them add it now and call `crm_list_pipelines` again. If they'd rather not add a post-call stage, note that calls on that pipeline will be flagged in the report instead of moved.
5. Ask about follow-up timing in plain words, one pipeline at a time: "For each stage, how many days of silence before someone goes on your follow-up list?" Show a suggested number next to each stage so they only change the ones they disagree with. Starting points: first contact 2 to 3 days; in conversation 3 to 5; call booked none (wait for the call); proposal sent 3 to 4; follow up or nurture 7 to 14.
6. Don't ask about custom fields. Call `crm_search_contacts` for any contact, then `crm_get_contact` on it, and read the field names yourself. The useful ones are LinkedIn URL, LinkedIn chat thread and a document folder link. If LinkedIn URL or LinkedIn chat thread is missing, say in one line that adding them (Settings, then Custom Fields) helps the check find LinkedIn chats, and that it works without them. If the CRM has no contacts yet, note which fields to add and move on.

## 4. Stage moves the check may make on its own

Propose these four, mapped to their stage names, and ask which to keep:

- A new booking or calendar invite for a discovery call: move to their call-booked stage.
- A call that has happened while the item is at call booked: move to their post-call stage (usually follow up). Only for pipelines that have one; for the rest, write that a call that has happened is flagged, not moved.
- They sent a proposal: move to their proposal stage.
- A lead from a discarded location: move to not qualified with a note.

Anything not listed is flagged in the report, never moved. Wins and losses are never automatic.

## 5. Where leads come from

For each connected lead source, read what's there and ask which pipeline and stage a new person goes into:

- **Quiz tool:** list their quizzes. Accounts often hold quizzes built for other people, so pre-sort them yourself into "looks like theirs and live" and "looks like someone else's or unused" (by name, brand and recent results). Show the first group for them to confirm, and ask only about the ones you can't place.
- **Booking tool:** list their event types. Ask which are sales calls and which pipeline each goes to. Take the booking link (the public scheduling URL, never an API address) from the main discovery-call event type.
- **Email marketing:** list their forms. Long lists are normal, so pre-sort them into "looks like a lead form" (enquiry, call, audit, offer or request forms, especially paid ones), "looks like a lead magnet or newsletter" (free downloads, newsletter, workshop sign-ups) and "can't tell". Show the first group and the "can't tell" group; take the lead-magnet group as not creating leads unless they say otherwise. For each lead form, ask which pipeline and stage a new person goes into: someone who paid (a paid audit or offer) is warmer than a free download and usually starts at a later stage.
- **Email:** ask what an enquiry looks like for them (replies to their offers, website contact form, referrals).
- **LinkedIn inbox:** positive replies to their outreach.
- Ask who to ignore: people pitching their own services, existing clients, suppliers, anyone else.

## 6. Where leads are

Ask which countries or regions they sell to, and which to discard. Anywhere else is added to the CRM but flagged, not drafted for. If they sell everywhere, leave the section out.

## 7. LinkedIn inbox (only if Kondo is connected)

1. Call `read_status`. If no Kondo tab is connected, ask them to open the Kondo web app in Chrome and keep it open, then try again.
2. Call `list_inbox` with limit 1. The `profileUrn` in the result is the user's own LinkedIn profile URN. Write it in the settings; never show it to the user.
3. Call `list_inboxes` and show their labels by name. Ask which labels mark pipeline stages (and which stage each means), and which labels are their own and must never be touched. If they don't use labels for stages, leave stage labels out.
4. Most chats fetched from LinkedIn per run: 5, unless they ask for fewer. Explain in one line that more risks their LinkedIn account being restricted.
5. Tell them plainly: the check can only read LinkedIn while a Kondo tab is open in Chrome at the time it runs.

## 8. What already happens automatically

If email marketing is connected, list their sequences and automations and ask which ones leads get automatically (for example a quiz follow-up sequence, or reminders to finish the quiz). Write each as one line so drafts never repeat them.

## 9. Other channels

Ask whether they talk to leads on WhatsApp, text or phone. The check can't read those; it lists those people as "check WhatsApp" instead of drafting.

## 10. Drafting

1. **Voice:** search their Google Drive for a voice profile (titles containing "Voice Profile" or "Voice"), or a voice skill in this chat. Confirm which one. If there's none, say drafts will be switched off until a voice profile exists, and leave the Today list and CRM updates on.
2. Which channels to draft on: email, LinkedIn, or both.
3. Most drafts per run: 5 unless they want fewer.
4. Email sign-off: usually none, because their email adds the signature. Ask.
5. Phrases never to use. Always include "just checking in", "just touching base", "just following up"; add any of theirs.

## 11. Clean-up, documents, never contact

- **Clean-up:** set to "preview only". Say it stays that way for at least the first week, and LinkedCoach switches it on with them once the previews look right.
- **Documents (optional):** do they keep a folder per prospect in Google Drive? If yes, get the folder (search Drive and confirm), and where client folders go once someone signs. Any sheets the check may read but must never change.
- **Never contact:** anyone the check must never draft for.
- **Anything else:** any rule of theirs that doesn't fit above.

## 12. Write the settings document

1. Write the whole document in Markdown, following `settings-template.md` section by section, with their real values and no example values left in. Leave out optional sections that don't apply, and the apps they don't use.
2. Create it in their Google Drive as a Google Doc: `create_file` with `contentMimeType` "text/markdown", the Markdown as `textContent`, and title "Pipeline Check Settings". If they named a folder, put it there.
3. Give them the link and a five-line summary: pipelines covered, lead sources, apps on, drafting on or off, clean-up preview only. Tell them they can edit the doc any time, and the next run picks the change up.

## 13. Test run

Offer a test run now, in this chat. It takes 5 to 10 minutes. Run the `pipeline-check` skill with the new settings document as a **test run that writes nothing**: no CRM writes, no drafts, no labels, no document changes, and no clean-up tools. Report what it *would* do in the normal report format, with "Would..." wording, and quote the first two lines of any draft it would write.

Go through the report with them. Fix anything wrong in the settings document straight away (edit the doc, then say what changed).

## 14. Schedule it

Ask what time it should run each weekday, in their time zone. Suggest a time when they're normally at their desk with Chrome open, so the Kondo tab is there.

If a scheduling tool is available in this chat, create a weekday scheduled task named "Daily pipeline check" with exactly this instruction, with the settings link filled in:

> Run my daily pipeline check with the pipeline-check skill. My settings are in this Google Doc: [settings link]. Nobody is watching this run, so don't stop to ask; put anything that needs me in the report. If the pipeline-check skill isn't available in this run, don't improvise the check: write a one-line report saying "Pipeline check didn't run: the pipeline-check skill wasn't available."

If there's no scheduling tool, give them those words and tell them to create the task from **Scheduled** in the sidebar.

Then tell them two things:

1. Open the task's settings and set approvals to automatic ("Automatically approve"). Without that, the run stops at its first CRM update, because nobody is there to approve it.
2. Every report ends with the line "Pipeline Check playbook" and a version number. If that line is missing, the check didn't run properly: tell LinkedCoach.

Finish with one line: what time the first run is, and that a Kondo tab needs to be open in Chrome then.
