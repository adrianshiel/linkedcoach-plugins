---
name: voice-review
description: Weekly review of the user's voice corrections log. Finds patterns seen at least twice, proposes changes to their Voice Rules document with full before-and-after examples, and applies only the changes the user approves. Use when a scheduled task or the user says weekly voice review, review my voice, voice review or update my voice rules, or asks for a rule they've just stated to go into their rules now.
---

# Voice review

You're reviewing the user's corrections log and proposing changes to their voice rules. The user approves every change. Nothing in the Rules doc changes without a yes from them in this conversation.

Before you start, read `skills/my-voice/references/drive.md` and `skills/my-voice/references/rules-template.md` in this plugin.

## 1. Read everything

1. Find the user's Voice folder, the Rules doc and the log, as described in drive.md. If any of them is missing, say which in one line, tell the user to say "set up my voice", and stop.
2. Read the Rules doc in full and every row of the log.

## 2. Work out the proposals

Only rows with status OPEN or STATED are candidates. Rows marked PROMOTED or REJECTED are settled. Use REJECTED rows to spot a pattern that has been turned down before, and say so if it comes back.

**The gate.** Only these become rules:

- **A stated rule** (status STATED). The user said it outright, so it needs no second sighting.
- **A pattern seen at least twice, in separate pieces of writing.** Group rows by the underlying pattern, not by surface wording. Rows about the same rule count together even if the words differ. Two changes in the same piece of writing count as one sighting.

Never propose a rule from one inferred correction. This gate is the point of the system: promote every one-off and within two months there are dozens of narrow, contradictory rules, and the writing gets worse.

**Also propose:**

- **A wider rule.** A pattern seen in two or more registers goes under "Everywhere".
- **A new register.** If the log names a register the Rules doc doesn't list, propose adding it to the Registers list as Not established, even after a single correction. Its rules follow the gate like any other.
- **Register status.** Suggest Not established to Thin once a register has 2 or more rules from real corrections. Suggest Thin to Established only when the user's corrections in that register have mostly stopped. The user decides.
- **Closing one-offs.** An OPEN row more than 8 weeks old with no second sighting is probably about that one piece. Propose marking it REJECTED.
- **Confirm or kill.** Ask about low-confidence rows that have sat for more than 4 weeks, 3 at most per review.
- **Fixes to the doc itself.** If two rules in the Rules doc contradict each other, propose a fix.

**Rows logged by someone else.** A row with "Logged by [name]" in Notes came from a team member writing in the owner's voice. Show these as their own group, one by one, with the full before and after. They never count as a sighting of the owner's own, and the gate doesn't apply to them. The owner approves or rejects each one, and an approved row becomes a rule like any other.

**Conflicts.** If a proposal contradicts an existing rule or another proposal, lay out the options and let the user choose. Never pick one yourself.

## 3. Present

Lead with the count, for example "3 proposed changes, 1 conflict, 2 one-offs to close." If there's nothing, write one line ("Nothing to change this week: 4 new corrections, all one-offs so far.") and stop.

Present at most 7 proposals in one review. The rest wait for next week. Show every proposal in the chat, never in a file. For each one, show:

- **The rule** exactly as it would appear in the Rules doc.
- **Where it goes** (the section).
- **The evidence:** the log IDs and dates, and the full before-and-after sentences from the log, not bare words. The user can't judge a word swap without the sentence around it.
- **Confidence,** and whether the sightings really describe the same thing or you're stretching to connect them.

Ask the user to approve, change or reject each one. Use AskUserQuestion if it's available, with one question per proposal and up to 4 per call. Otherwise number the proposals and ask for a reply such as "1 yes, 2 no, 3 change it to...".

**In a scheduled run,** present the proposals and wait for the user's answers in this chat. Change nothing until they answer. If they never answer, nothing changes, and next week's review proposes the same things again.

**At the end of the report,** add the line "My Voice review 0.3.0". It shows the review ran from the plugin.

## 4. Apply what's approved

1. **Write the new Rules doc in full.** Check the current text you read ends with the "Default rules switched off" heading. If it doesn't, the read was cut short: stop and tell the user. Start from the current text, make only the approved changes, and keep every other line exactly as it is. Update "Last updated". Remove any `**` markers that the read added around headings.
2. **Keep the rules clean.** Write the rule, not its history: no dates and no "added because". Add a reason only when it stops a known mistake recurring.
3. **Replace the doc** using the replacing steps in drive.md. If the checks fail, nothing changes. Tell the user and stop.
4. **Update the log.** Set approved rows to PROMOTED. Set amended rows to PROMOTED with "amended" in Notes. Set rejected rows to REJECTED with the reason in Notes. In every case, set Status date to today. Rows you didn't propose stay as they are.
5. **Confirm in two lines:** what changed, and the link to the Rules doc.

**When the doc gets long.** The Rules doc has a limit of 27,000 characters (about 4,500 words), because Drive's reader cuts longer documents off (see drive.md). Past 24,000 characters, propose merging overlapping rules and cutting rules the default rules already cover. If an approved change would take the doc over the limit, don't apply it until the user has approved enough condensing to make room.

## The user's own edits

The user can edit the Rules doc by hand at any time. Treat what you find there as approved. Never undo it, and never propose reversing it unless a later correction contradicts it.
