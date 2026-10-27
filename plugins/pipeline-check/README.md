# Pipeline Check

A daily check of your sales pipeline, run by Claude on a schedule. Each run:

1. Reads your CRM pipelines and open tasks.
2. Catches new leads from your quiz, booking tool, email, forms and LinkedIn, and adds anyone missing.
3. Reads every channel for every open lead before deciding anything: CRM notes, email, LinkedIn (including archived chats), email marketing, meeting notes, calendar and bookings.
4. Logs notes and tasks, and moves stages only on clear evidence.
5. Drafts up to 5 replies in your voice, in your email or LinkedIn inbox. It never sends anything.
6. Reports back: who to follow up with today, what it changed, what needs your decision, and anything it couldn't check.

## What you need

- A Claude Pro plan or above.
- Less Annoying CRM, with an API key (Settings, then Programmer API).
- The apps you use for leads connected in Claude (for example Gmail, Google Calendar, Calendly, ScoreApp, Kit, your meeting-notes tool, and Kondo for LinkedIn).
- Your settings document and voice profile. LinkedCoach builds both with you during setup.
- For LinkedIn through Kondo: a Kondo tab open in Chrome when the check runs. Make opening it part of your daily pipeline routine.

## Set it up

1. Add the LinkedCoach marketplace, then install this plugin from **Browse plugins** (see the main README). Adding the marketplace on its own doesn't install the plugin. In Claude Code the same step is:

   ```
   /plugin install pipeline-check --marketplace adrianshiel/linkedcoach-plugins
   ```

2. Open the plugin in **Customize, Plugins**, go to its **Connectors** tab and connect **less-annoying-crm**. On the sign-in page, paste your Less Annoying CRM API key and choose **Allow**.
3. Start a new chat and say **"set up my pipeline check"**. Claude reads your CRM and apps, asks you to confirm the details, writes your settings document into your Google Drive, runs a test that changes nothing, and creates the scheduled task.
4. In the scheduled task's settings, set approvals to automatic. Without that the run stops at its first CRM update.

Every report ends with "Pipeline Check playbook" and a version number. If that line is missing, the check didn't run properly.

## Your data

The CRM connector runs at crm.linked.coach. Your CRM key is checked with Less Annoying CRM when you connect, then stored encrypted inside your own sign-in. It is only used to reach your own CRM, and is never shared with other clients. To cut access off at any time, delete the key in Less Annoying CRM.

The check reads the apps you connect in Claude, using your own connections. It writes notes, tasks and stage changes to your CRM, and drafts to your email and LinkedIn inbox. It never sends a message and never deletes anything except through three guarded clean-up tools, which only preview until your settings allow them.
