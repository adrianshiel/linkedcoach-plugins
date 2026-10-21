# How to read each app

Use only the apps the settings switch on. Each section says what to look for and what never to do.

## Email (Gmail or Outlook)

- Search threads to or from each lead, including threads where they're only copied in.
- Meeting notes that arrive by email (for example Google Meet notes from gemini-notes@google.com) count as meeting notes: read them.
- New enquiries: replies to the user's offers, website enquiries, partner enquiries. Skip newsletters, notifications and suppliers.
- Payment emails: search for the providers the settings list (for example Stripe, PayPal, Payfunnels, Mercury) for anyone at a proposal stage.
- Drafts: check existing drafts before creating one, so there's never a second draft for the same person. If a draft contains a link that starts with a redirect (google.com/url, safelinks), say the link needs replacing.

## Calendar

Events with pipeline contacts in the last 3 days and next 2 days. Many calls are booked by direct invite rather than the booking tool, so always check the calendar too.

## Booking tool (Calendly or similar)

Bookings created in the last 3 days and calls in the next 2 days. List invitees to get names, emails and booking-form answers. The settings say which pipeline a booking goes to, often based on a booking-form answer.

## Quiz tool (ScoreApp or similar)

Results from the quizzes the settings list, and only those. Most accounts also hold quizzes that belong to someone else; ignore them. The quiz tool's connector may not return the lead's country; if a browser is available, the lead's page in the app usually shows it.

## Email marketing (Kit or similar)

- Look each lead up by email. Note tags, location, what they've been sent, and whether they opened or clicked. Treat opens and clicks as signs of warmth, but an open within a few seconds of sending is an automatic scanner, not a person.
- Form sign-ups: for each form the settings list, check subscribers added in the last 3 days and place them as the settings say.
- Read what the settings say the email marketing already sends automatically. Never draft anything that repeats it (for example resending a quiz link the reminders already sent).

## Meeting notes (Wispr Flow, Fathom, Otter, Fireflies or similar)

Search by attendee email or name. Read the notes, and the transcript if the notes are thin. A meeting that happened is evidence for a stage move if the settings have a rule for it.

## LinkedIn inbox (Kondo)

Kondo works only while the user has a normal browser tab of the Kondo web app open. If a call says no tab is connected, wait about a minute and try once more. If it still fails, write one line under "Couldn't check": "LinkedIn inbox wasn't reachable at HH:MM." Don't explain setup.

Kondo's stored messages are often empty, and then a search by name finds nobody even when the chat exists. Look chats up in this order:

1. If the CRM contact has a "LinkedIn chat thread" field, take the thread key from it (the part after /thread/, without the trailing slash) and call `read_chat` with the user's LinkedIn profile URN from the settings and that key.
2. Otherwise try `list_chats` by name (view everything), then by their LinkedIn profile URL. When you find the chat, save its thread URL to the contact's "LinkedIn chat thread" field with `crm_update_contact`, if the CRM has that field.
3. To see what's new without fetching every chat from LinkedIn, call `list_inbox` for "all", "archive" and "reminder" (limit 50 each) and compare each pipeline chat's latest activity with the latest contact already logged in the CRM. Many users archive a chat or set a reminder after every interaction, so pipeline chats are often there.
4. If `read_chat` comes back empty or with only one or two messages, and the chat has activity newer than the CRM's last note (or you need the history to draft), use `load_chat`, then `read_chat` again.
5. Messages the user sends directly in LinkedIn can be missing from Kondo until the chat is fetched. So before drafting for someone on LinkedIn, and before listing someone as waiting on the user's reply, run `load_chat` and `read_chat` again, even if the stored copy looks complete.

Also use `list_inbox` with an "after" time and the views for chats needing the user's reply and chats awaiting the other person's reply, to catch new people.

**Protect the user's LinkedIn account.** Some Kondo tools call LinkedIn directly, and too much of that can get the account restricted.

- Read from stored messages with `list_chats` (without `linkedin: true`), `list_inbox` and `read_chat`.
- Use `load_chat` only as above, and never more than the limit in the settings (5 if not set). If you need more, list the people you couldn't load under "Couldn't check" and don't draft for them on LinkedIn.
- Never use `list_connections`, `list_invites`, `set_invite_response`, `set_chat_archived`, or `list_chats` with `linkedin: true`.

**Drafts:** use `set_chat_draft`, only if the chat has no draft already (check the draft inbox). Kondo can't show the text of a saved draft: if a chat is in the draft inbox, don't draft for that person on any channel and say "there's an unsent draft in [name]'s LinkedIn chat that I couldn't read". If the person isn't a 1st-degree connection there's no chat to draft in: say so and suggest a connection request.

**Labels:** only if the settings define stage labels. Call `list_inboxes` once per run to get label keys by name; never hard-code them. Use `set_chat_label` with "add" or "remove", never "replace". Only label the chat of someone in the CRM pipeline, and only after reading that chat in this run. A chat carries at most one stage label: when the stage changes, remove the old one and add the new one. Never add or remove the labels the settings list as the user's own. If a label you need doesn't exist, don't create it; say so under "Needs your decision". List every label change under "Updated in the CRM".

## LinkedIn in a browser

Only if browser tools are available in the run. Use it to check a lead's profile, headline, location, recent activity and website. Read only: never send messages, connection requests, likes, comments or follows, and keep it to the profiles you need (about 20 a run at most). Work in a new tab and leave the user's tabs alone. If no browser is available, say so in one line under "Couldn't check".

## Documents (Google Drive or similar)

If the settings name a folder for prospect work: use a lead's folder link from the CRM if there is one, otherwise search the folder by name, and save the link back to the CRM when you find it. A proposal, mockup or report in the folder backs up the matching stage. A folder moved to the clients folder suggests they've become a client, but only suggest a win once the money has arrived.

## Read-only sheets

Some settings list a sheet the run should read but never change (for example an outreach log filled by another tool). Read it once per run, use it only as a pointer to people worth checking in their actual chat or inbox, and never act on the sheet alone.
