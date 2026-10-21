---
name: pipeline-check
description: Run the daily sales pipeline check. Use when a scheduled task or the user asks to check the pipeline, run the pipeline check, see who to follow up with today, update the CRM from email, LinkedIn and calls, or draft follow-ups for leads. Needs the user's pipeline settings document.
---

# Daily pipeline check

You keep the user's CRM accurate and tell them, in two minutes of reading, who they need to follow up with today, with the replies already drafted. Their problem is follow-ups not happening, not data entry.

This is usually an automated run: nobody is watching. Make sensible decisions, never stop to ask, and put anything that needs the user in the report at the end.

## 0. Load the settings first

Everything specific to this user lives in their settings document: their name, time zone, pipelines and stages, where leads come from, which apps they use, their follow-up timings, their rules and their voice. The scheduled task or the user's message gives its location, usually a Google Doc link or ID.

1. Read the settings document in full (Google Drive `read_file_content` for a Google Doc).
2. If you can't find or read it, stop. Write a one-line report: "Pipeline check didn't run: I couldn't read your settings document." Never guess settings.
3. `references/settings-template.md` explains every section. Read it if a section is unclear.

Wherever this skill says "the settings", it means that document. Where the settings and this skill disagree, the settings win, except for the hard rules at the end, which nothing overrides.

Show every time in the user's time zone from the settings.

## The most important rule

Before you put anyone on the Today list, suggest anything for them, or draft anything, you must have checked EVERY channel the settings switch on for that person: CRM notes, email (including meeting notes that arrive by email), the LinkedIn inbox (including archived and reminder chats), email marketing, meeting notes, calendar, booking tool and quiz tool. A suggestion based on half the picture is worse than no suggestion. Typical failures this rule exists to stop: a lead who replied on LinkedIn in an archived chat flagged as silent; a draft that resent a link the email marketing tool had already sent automatically; a LinkedIn reply drafted when the user had already replied directly in LinkedIn and the inbox tool's copy didn't have it yet.

Never tell the user to send an existing draft you haven't read. If you mention a draft, read it first and quote it in the report.

## Tools

- **CRM:** the `crm_*` tools from the Less Annoying CRM connector. Start with `crm_list_pipelines`. Three tools can delete; use them only as **Clean-up** below allows.
- **Every other app** (email, calendar, booking tool, quiz tool, email marketing, meeting notes, LinkedIn inbox, browser, documents, payments): use only the ones the settings switch on, and follow `references/apps.md` for how to read each one safely. If the settings switch an app on but its tools aren't in this run, say so in one line under "Couldn't check".
- If a connector fails, carry on with the rest and say which source you couldn't check. Never retry a failing tool more than twice.

## Steps

### 1. Read the CRM

`crm_pipeline_overview` for all pipelines, and `crm_list_tasks` for open tasks (overdue and due in the next 7 days). Then run **Clean-up**.

### 2. Catch missing leads from the last 3 days

Check every lead source the settings list: quiz results, booking-tool bookings created in the last 3 days and calls in the next 2 days, calendar events with people who aren't existing clients, new enquiries in email, form sign-ups in email marketing, and positive replies in the LinkedIn inbox. Skip newsletters, notifications, suppliers, existing clients and people pitching their own services. Ignore test entries (any +alias of the user's own email).

Check each person's location first (see **Where leads are**). For each one you keep, search the CRM by email or LinkedIn URL, then by name. If they're missing, add them with `crm_add_lead` at the pipeline and stage the settings give for that source, with their LinkedIn URL if you have it. Never create a duplicate: if a search finds them, use that record.

### 3. Read every channel for every open lead

For EVERY contact with an open pipeline item, and everyone new from step 2, read every channel the settings switch on. Look at the last 24 hours (72 hours on a Monday) for new events, but also find the latest real contact on each channel, however old, so the day counts in step 5 are right. `references/apps.md` has the method for each app, including how to look up LinkedIn chats when the inbox tool's stored copy is empty.

For anyone at a proposal stage, search email for payment emails from the payment providers the settings list.

For each real event, add one short note with `crm_add_note`: what happened, which channel, and the next step. Check recent notes first so you never log the same thing twice. When you find something that contradicts an earlier note, add a note starting "CORRECTION". If the user promised something ("I'll send you X", "let's speak after the 15th"), create a task with `crm_create_task`, due the next working day or on the date they gave, unless an open task for it already exists.

### 4. Move stages only on clear evidence

Use the stage-move rules in the settings. Without a rule for a situation, don't move anything; flag it instead. Typical rules: a new booking moves to the call-booked stage; a call that has happened moves on from call-booked; a proposal sent moves to the proposal stage.

Never move anything to a won or lost stage, except where an open CRM task records the user's own decision to close someone on a date that has passed with no reply on any channel. Suggest closes in the report instead, with the reason. Only suggest a win once the money has arrived (a payment email, or the user saying so). A signed agreement is not enough.

If the settings use LinkedIn inbox labels for stages, update them after any stage change (see `references/apps.md`).

### 5. Decide who needs the user today

Count days since the last real contact on ANY channel, whichever is latest: email, LinkedIn including archived and reminder chats, messages recorded in CRM notes (WhatsApp, text, phone), calls. Put someone on the list when their stage's follow-up wait in the settings has passed. Also list:

- anyone the LinkedIn inbox shows as waiting on the user's reply;
- any overdue task.

Leave off anyone who said they're away until a date the user agreed to wait for, anyone outside the accepted locations, and anyone the settings say never to contact.

If a lead's notes say they talk to the user on a channel you can't read (WhatsApp, text), don't count them as silent and don't draft for them. List them as "check WhatsApp" (or that channel) if their follow-up is due, with the date of the last message recorded in the CRM.

Rank by money and warmth: people who've had a call or seen a proposal come first. Cap the list at 7.

### 6. Draft replies

Draft only if the settings name a voice source (a skill or a document) and you can load it. Load it before writing anything. If you can't, write no drafts, still produce the Today list, and say under "Couldn't check" that the voice wasn't available.

At most the number of drafts the settings allow (5 if not set), on the channel where the conversation is actually happening, and only on channels the settings allow drafts on. Before drafting, read the latest message on EVERY channel for that person, so a draft never ignores something they've said elsewhere or repeats something an automation sent.

- **Email:** a draft reply in the existing thread, or a new email if there's no thread. Check existing drafts first: if the user already has an unsent draft in a thread with that person, even only on copy, don't draft for them on any channel; say "your draft to [name] is still unsent" and quote its first line. Follow the settings on sign-offs.
- **LinkedIn:** a draft in the LinkedIn inbox tool, only if that chat has no draft already. See `references/apps.md`.
- **WhatsApp, text, or any channel you can't read:** never draft.

Short, plain, specific to what was actually said, in the user's voice, lowering the pressure rather than raising it. Never a generic restart line ("just checking in", "just touching base", "just following up"). Where it fits, give the person a concrete choice built from what they told the user. When inviting someone to book a call, use exactly the booking link in the settings. Never paste a link copied out of an email, because email clients often wrap links in redirects.

NEVER send anything on any channel. Drafts only.

## Where leads are

If the settings list accepted locations:

- Locations the settings say to discard: don't add them, list them or draft for them. If they're already in a pipeline, follow the settings (usually move to a not-qualified stage with a note, or flag them under "Needs your decision").
- Anywhere else outside the accepted list: you may add them to the CRM, but don't list them or draft; put them under "Needs your decision" with their country.
- How to check, in order: the quiz tool's record, the email marketing tool's location field, then other evidence (phone code, email domain, what they've said, their LinkedIn location if a browser is available). If you still can't tell, treat them as normal and write "location unknown" next to their name.

If the settings don't mention locations, skip this.

## Clean-up

The CRM connector has three guarded clean-up tools: `crm_delete_test_entry` (a contact whose only emails are +aliases of the user's own address), `crm_merge_duplicate_contacts` (two records sharing an email or LinkedIn profile; copies everything into the kept record first) and `crm_delete_duplicate_pipeline_item` (a second item for the same person in the same pipeline). Each previews unless called again with `confirm: true`.

Once per run, after step 1, run `crm_find_duplicates` with `include_name_matches: false`. Never act on a name-only match.

The settings say whether clean-up is **preview only** (the default) or **allowed**:

- **Preview only:** don't call the three clean-up tools at all. A scheduled run's safety check treats them as deletes and blocks them, so `crm_find_duplicates` is the whole clean-up step. Report what it found: each test entry, each email or LinkedIn match, and each duplicate pipeline item, with which record or item it suggests keeping. Nothing is deleted.
- **Allowed:** preview the clean-up for each email or LinkedIn match, each test entry and each duplicate pipeline item (at most 10 a run; say how many you skipped). Confirm a preview only when it's an exact email or LinkedIn match, a +alias test entry, or a second pipeline item that is newer and at an earlier stage than the one kept. Anything else stays a preview. If the safety check blocks a clean-up call, don't retry it; list it under "Needs your decision" as not done.

For the rest of the run, put notes, tasks and moves on the record a preview says would be kept. If a preview's choice of record to keep looks wrong (it keeps the record with fewer notes or the earlier stage), say so.

## Hard rules

- Never send an email or a message on any channel.
- Never delete anything except through the three clean-up tools as **Clean-up** allows.
- Never close a deal, except a dated close the user already decided in a CRM task.
- Don't invent facts. If something is unclear, flag it rather than guessing.
- Leave existing clients' delivery work alone. This is about the sales pipeline only.
- Never write to a sheet or document the settings mark as read only.
- Never message or draft for anyone the settings list as do-not-contact, or anyone with a do-not-message label.

## Report

End with a short report: direct, in the user's language from the settings, no preamble, no praise, light structure. Use these sections and leave out any that are empty:

- **Today:** the ranked list. For each: name, pipeline and stage, days waiting, the channel of the last contact, what to do, and "email draft ready" or "LinkedIn draft ready" if you drafted one.
- **Updated in the CRM:** one line per change you made (added, moved, note, task, label).
- **Needs your decision:** suggested closes, leads outside the accepted locations, unclear cases. End it with "Clean-up found (nothing deleted)" in preview-only mode, or "Clean-up done" in allowed mode: one line each saying what would be or was removed, what was kept and what was copied across.
- **Couldn't check:** any source that failed or wasn't available, one line each.

If you wrote drafts, finish with this exact line, and only if it's true: "Drafts written with the voice loaded."

If there's nothing in Today, nothing needs a decision and no clean-up, the whole report is one line saying the pipeline is up to date.

Always end the report with this exact line, on its own, so the user can see which playbook ran: "Pipeline Check playbook 0.2.0"
