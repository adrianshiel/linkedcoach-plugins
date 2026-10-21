# Pipeline check settings

Each user keeps one settings document, usually a Google Doc in their own Drive. The setup interview writes it; the user can edit it any time. The run reads it at the start of every check.

Below is every section, with what it means. Sections marked optional can be left out, and the run then skips that app or rule. The example values are for a fictional coach, Jane.

---

## About me

- Name: Jane Example
- Business, in one line: executive coaching for new leadership teams
- Time zone: America/New_York
- Report language: English (US)
- My own email address: jane@janeexample.com (its +aliases are test entries)

## CRM

- CRM: Less Annoying CRM, through the shared connector
- Custom fields that exist: LinkedIn URL, LinkedIn chat thread, Google Drive Folder Link (only list the ones you've created)

## Pipelines

For each pipeline: its name, its stages in order, which stages are won, lost or not qualified, and how many days to wait at each open stage before Jane should follow up.

**Sales**

| Stage | Follow up after |
| --- | --- |
| New enquiry | 2 days with no message from Jane |
| In conversation | 5 days |
| Call booked | (none, wait for the call) |
| Proposal sent | 4 days with no reply |
| Follow up | 7 days |
| Won | closed |
| Lost | closed |
| Not qualified | closed |

## Stage moves the run may make on its own

One line per rule. Anything not listed is flagged, not moved.

- A new booking or calendar invite for a discovery call: move to Call booked.
- A call that has happened while the item is at Call booked: move to Follow up.
- Jane sent a proposal (email subject or body says proposal, usually with an attachment): move to Proposal sent.
- A lead outside the accepted locations who is in the Sales pipeline: move to Not qualified with a note.

## Where leads come from

One line per source: the app, what to look for, and which pipeline and stage a new person goes into.

- Quiz (ScoreApp): "Leadership Readiness Scorecard" only. New finishers go to Sales at New enquiry.
- Booking tool (Calendly): discovery call bookings go to Sales at Call booked.
- Email enquiries (Gmail): replies to Jane's offers and website enquiries go to Sales at In conversation.
- Email marketing form (Kit): "Free Strategy Session" form. New sign-ups go to Sales at New enquiry.
- LinkedIn (Kondo): positive replies to Jane's outreach go to Sales at In conversation.
- Ignore: anyone pitching their own services, existing clients, suppliers.

## Where leads are (optional)

- Accepted: United States, Canada, United Kingdom, Ireland, Australia, New Zealand, Europe
- Discard: (list any countries or regions to drop)
- Anywhere else: add to the CRM but flag, don't list or draft

## Apps switched on

Tick the ones Jane uses. Anything not ticked is skipped.

- [x] Email: Gmail
- [x] Calendar: Google Calendar
- [x] Booking tool: Calendly
- [x] Quiz tool: ScoreApp
- [x] Email marketing: Kit
- [x] Meeting notes: Fathom, and Google Meet notes from gemini-notes@google.com
- [x] LinkedIn inbox: Kondo (Business plan; a Kondo tab must be open in Chrome during the run)
- [ ] LinkedIn in a browser
- [ ] Documents folder
- [x] Payment emails from: Stripe

## LinkedIn inbox details (if Kondo is on)

- My LinkedIn profile URN: 123456789
- Most chats Jane fetches from LinkedIn per run: 5
- Stage labels the run manages (Kondo label name, then CRM stage):
  - Awaiting reply: Jane has made a first approach and they haven't replied yet
  - In conversation: a real back-and-forth, no call offered yet
  - Call booked: Call booked
  - Won / Lost: only once the CRM item is closed
- My own labels the run never touches: Hot lead, DO NOT MESSAGE, Other

## What already happens automatically

So drafts never repeat it.

- Kit sends quiz finishers a 5-email sequence over 10 days.
- Kit sends 2 reminders to anyone who starts the quiz and doesn't finish. Never resend the quiz link.

## Other channels I use with leads

- WhatsApp with some leads. Their CRM notes say so. The run can't read it and never drafts for it.

## Drafting

- Voice: the "jane-voice" skill (or: the Google Doc "Jane Voice Profile")
- Drafts on: email, LinkedIn
- Most drafts per run: 5
- Booking link: https://calendly.com/jane-example/discovery
- Email sign-off: none; Gmail adds my signature
- Never write: "just checking in", "just touching base", "just following up"

## Clean-up

- Mode: preview only (or: allowed)

## Documents (optional)

- Prospect folders: Google Drive, Shared drives > Sales > Prospects (one folder per person)
- When someone becomes a client, their folder moves to: Shared drives > Clients
- Read-only sheets: (any sheet the run may read but never change, and what it's for)

## Never contact

- (names or emails of anyone the run must never draft for)

## Anything else

Free text for rules that don't fit above. The run follows these as long as they don't break the hard rules.
