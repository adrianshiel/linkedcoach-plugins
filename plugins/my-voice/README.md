# My Voice

Claude writes in your voice, logs every correction you make, and once a week reviews the log with you so your rules keep improving. Nothing in your rules changes without your yes.

## What's in it

- **my-voice:** loads your rules before any writing, picks the right register, applies the default anti-AI rules, and logs your corrections.
- **voice-review:** the weekly review. It finds patterns you've corrected at least twice, shows you the before and after, and updates your rules with the ones you approve.
- **voice-setup:** one-time setup. It builds your rules from your voice profile, creates your log, and schedules the weekly review.

## What you need

- Google Drive and Google Sheets connected in Claude.
- Your voice profile, or 3 to 5 pieces you wrote yourself.

## Getting started

Install the plugin, start a new chat and say "set up my voice".

## Writing in someone else's voice

A team member who writes for you (replying to your LinkedIn messages, say) uses your rules directly, so there's no copy to keep in sync.

1. Share your My Voice folder with them as a viewer. To have their edits to your drafts logged for your weekly review, also share My Voice Corrections Log with them as an editor.
2. They install this plugin and connect Google Drive and Google Sheets. They don't need to run setup.
3. They ask Claude to write as you, for example "reply to this message as Adrian". Claude reads your current rules every time, so any change you approve reaches them straight away.

## Your files

Your files live in your Google Drive, in a folder called My Voice:

- **My Voice Rules:** your rules, which you can edit by hand at any time.
- **My Voice Corrections Log:** one row per correction.
- **Old versions:** a copy of your rules from before each change.
