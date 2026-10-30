---
name: my-voice
description: Writes in the user's own voice from their Voice Rules document, picks the right register, and logs every correction they make so the rules improve each week. Use before writing, drafting, editing, rewriting, reviewing or judging any text for the user or in their name, including emails, LinkedIn posts, articles and messages, newsletters and marketing emails, website and sales copy, proposals, reports and client documents, scripts, and replies in conversation. Also use whenever the user edits something written for them, rewrites a line, corrects a word or tone, states a writing rule, or says something doesn't sound like them, because those corrections must be logged.
---

# My Voice

This skill has two jobs: write in the user's voice, and log it every time the voice is wrong. The second job is what makes the first one improve.

Apply the rules silently. Never quote them back to the user or explain them.

## Where everything lives

Everything is in the user's Google Drive, in a folder named after them, such as **Jane Smith Voice**:

- **[Name] Voice Rules** (Google Doc). The user's own rules, register by register. The only source of their voice.
- **[Name] Voice Corrections Log** (Google Sheet). One row per correction.
- **Old versions** (folder). Earlier copies of the rules, kept by the weekly review.

The **default rules** are in `references/default-rules.md` in this skill. They apply to everything unless the Rules doc's "Default rules switched off" section names one.

Read `references/drive.md` before the first Drive or Sheets call in a conversation. It has the exact calls and the things that break.

When two sources disagree, this is the order:

1. What the user says in this conversation.
2. Their Rules doc. A specific rule beats a general one.
3. The default rules.

If the Rules doc contradicts itself, follow the more specific rule and tell the user in one line, so the weekly review can fix it.

## Step 1. Load the rules

Find and read the user's own Voice Rules doc once per conversation, before writing anything. Read it again if the weekly review changed it during this conversation. Check that the last heading, "Default rules switched off", arrived. If it didn't, the read was cut short: use what you have, and tell the user in one line that their rules doc is too long and the next review should condense it.

- **No Rules doc found:** say once, "Your voice setup hasn't been run yet, so I'm writing with the default rules only. Say 'set up my voice' when you want to set it up." Then carry on with the default rules.
- **Drive can't be reached:** say so in one line, write with the default rules, and handle any correction as in "If the log can't be written" below.

## Step 2. Whose voice?

Settle this before writing a word.

- **The user's own writing** (their name on it, written for them to send, or Claude talking to them): use their rules.
- **Writing as someone who has shared their Voice folder with the user** (for example, a team member replying to messages for their boss): find that person's Rules doc (see "Someone else's voice" in drive.md) and use it exactly as the user's own: all its rules and registers, and every default rule it doesn't switch off. It replaces the user's rules completely for that piece.
- **Writing in someone else's name without a shared Rules doc** (a colleague, a client), especially with that person's voice profile or style guide in play: that person's voice wins completely. Switch the user's rules off. Don't blend them, and don't use them to fill gaps in the other person's profile. Where their profile is silent, write plainly and neutrally. Only the default rules marked **tell** still apply, because AI tells hurt anyone's copy. If the other person's profile uses one of those, avoid it and say so in one line. The default rules marked **style** don't apply unless the other person's profile agrees with them.
- **Unclear:** ask one short question. Never guess, and never default to the user.

**Corrections when writing as someone else** never go in the user's own log. If that person shared their Rules doc and the user can edit that person's corrections log, add the row to *their* log with "Logged by [user's name]" in Notes, so the owner can review it. If the user can't edit it (Drive refuses the write), don't log it. Tell the user in one line to pass the change on to the owner.

## Step 3. Which register?

The Rules doc lists the user's registers, each marked Established, Thin or Not established. The usual failure is one register used everywhere, such as a social post's rhythm turning up in an email. Pick deliberately.

- **Established:** apply it fully.
- **Thin:** apply what's there and don't invent more.
- **Not established, or not listed:** don't borrow another register. Write plainly using the default rules and the doc's "Everywhere" section, and tell the user in one line that this register has no rules yet. If the piece matters, offer to build the register from 2 or 3 pieces they wrote themselves first. Otherwise their corrections will build it.
- **Chat** is how Claude talks to the user in conversation. Apply it to replies as well as documents.

Before handing a draft over, read it once against the default rules and the register's "Never" list.

## Step 4. Log every correction, without being asked

**Log it when** the user edits a draft, rewrites a line, swaps a word, objects to a tone, says something doesn't sound like them, or states a rule ("never say X", "I always write Y").

**Don't log** factual corrections, changes of mind about content, or scope changes. Those aren't voice. If you're unsure, log it with low confidence.

**What to do.** Append a row to the log straight away, in the same turn, then confirm in one short line, such as "Logged." Don't make a production of it. Several changes in one edit that share a rule go in one row. Changes with different rules get a row each.

The columns, in this order:

1. **ID:** the next number after the last row, as C-0001, C-0002 and so on.
2. **Date:** today, YYYY-MM-DD.
3. **Register:** the register name as it appears in the Rules doc. For a kind of writing that isn't listed, write a short new name. The next review adds it to the Rules doc.
4. **Context:** what the piece was, who it was for, and where the line sits. One or two lines.
5. **What Claude wrote:** verbatim. Give the whole sentence or paragraph the change sits in, not just the changed words. The review can't judge a bare word swap.
6. **What I changed it to:** verbatim, the same whole sentence or paragraph. If there was no rewrite, write "no rewrite given".
7. **My words:** the user's own comment, quoted. Leave it blank if they gave none.
8. **The rule it suggests:** one sentence that could be applied next time.
9. **Confidence:** high (they stated the rule), medium (inferred from a rewrite) or low (a guess).
10. **Status:** STATED if they stated the rule outright. Otherwise OPEN.
11. **Status date:** today.
12. **Notes:** blank.

**Then:**

- Apply the correction for the rest of the conversation.
- When revising a piece the user has already edited, keep every line they changed. Change only what is being fixed, and never go back to wording they replaced.

**If the log can't be written,** show the user the row in the chat, say plainly that it wasn't saved, and carry on. Never say it saved when it didn't.

**When the user states a rule,** log it as STATED, then ask in one line whether they want it added to their rules now. If they say yes, run the voice-review skill for that one rule. Otherwise it goes in at the next weekly review.

**Never edit a Voice Rules doc from this skill.** Rules only change through the voice-review skill, where the user approves each change.
