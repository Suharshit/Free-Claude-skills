---
name: log-notion
description: Logs the work done in the current chat to the user's Notion - finished and pending tasks, project progress, docs and a dated session log, all linked to each other. On first use it sets up a Work Home page with three linked databases (Projects, Tasks & Reminders, Docs & Notes), or reuses ones that already exist. Use when the user types /log-notion or asks to log, save or record this chat's work, tasks or progress in Notion.
license: Free to use for your own work. Please don't resell it.
metadata:
  author: Suharshit Singh
  version: "1.0.0"
---

# Log this chat to Notion

Turn everything done in the current conversation into clean, linked records in the user's Notion: what was asked, what was built or decided, what was produced, and what is still left. Do the whole job without asking, unless a rule below says to ask.

## Tools

Use the Notion connector. If its tools are deferred, load them first. The ones this skill needs: `notion-search`, `notion-fetch`, `notion-get-self`, `notion-create-pages`, `notion-update-page`, `notion-create-database` (setup only) and `notion-query-data-sources`.

If no Notion tools are available in this chat, say: "Connect Notion first: Settings → Connectors → Notion, then run /log-notion again." Then output the log as text (project, done, pending, docs, progress) so nothing is lost.

## Step 0: Find the workspace (every run)

1. **Owner.** Call `notion-get-self` and keep the user's ID for every `Owner` field. If it fails, leave `Owner` empty.
2. **Find the databases.** Search Notion for a page titled "Work Home" and for databases titled "Projects", "Tasks & Reminders" and "Docs & Notes".
   - If all three databases exist and are related to each other (Tasks and Docs both have a `Project` relation to Projects), use them, even if they live somewhere other than Work Home.
   - Fetch each one before writing, and use its exact property names and option names from the fetch result. If a property is named differently (e.g. "Name" instead of "Task"), use theirs. Never rename or delete the user's properties.
3. **Nothing found → first-run setup.** Ask once:
   > "I'll set up a **🧠 Work Home** page in your Notion with 3 linked databases: Projects, Tasks & Reminders, and Docs & Notes. OK?"

   Wait for a yes, then follow "First-run setup" below. If they say no, output the log as text instead.
4. **Partial match** (e.g. a "Projects" database exists but no Tasks database, or several candidates): don't guess. Tell the user what you found in one or two lines and ask which to use, or whether to create a fresh Work Home.

## First-run setup

1. Create a private page titled **🧠 Work Home** (no parent, so it sits at the workspace top level). Body:
   > Everything Claude logs with /log-notion lives here: projects, tasks and docs, all linked.
   >
   > Set up by the log-notion skill on [today's date].
2. Create the three databases with `notion-create-database`, each with `parent` = the Work Home page ID, in this order (each later one needs the Projects data source ID from the first result's `<data-source>` tag):

   **Projects**
   ```sql
   CREATE TABLE ("Project" TITLE, "Status" SELECT('Idea':gray, 'Active':blue, 'Paused':yellow, 'Done':green), "Area" SELECT('Client':orange, 'Product':purple, 'Career':blue, 'Learning':green, 'Personal':pink), "Progress" NUMBER FORMAT 'percent', "Next step" RICH_TEXT, "Owner" PEOPLE, "Deadline" DATE)
   ```

   **Tasks & Reminders** (replace `PROJECTS_DS` with the Projects data source ID)
   ```sql
   CREATE TABLE ("Task" TITLE, "Status" STATUS('To do':gray:to_do, 'Doing':blue:in_progress, 'Done':green:complete), "Due" DATE, "Priority" SELECT('High':red, 'Medium':yellow, 'Low':gray), "Type" SELECT('Task':default, 'Reminder':purple, 'Follow-up':orange), "Project" RELATION('PROJECTS_DS', DUAL 'Tasks'), "Owner" PEOPLE)
   ```

   **Docs & Notes**
   ```sql
   CREATE TABLE ("Title" TITLE, "Type" SELECT('Doc':blue, 'Meeting':orange, 'Idea':yellow, 'Note':gray, 'Client':purple), "Project" RELATION('PROJECTS_DS', DUAL 'Docs'))
   ```
3. Fetch each new database once to confirm the property names, then continue with Step 1.
4. In the final report, add one line: "Set up 🧠 Work Home in your Notion. Find it in the sidebar; you can move it anywhere."

If a setup call fails partway, don't create duplicates on retry: search for what already got created and continue from there.

## Step 1: Extract from the conversation

Before touching Notion, make a short internal list:

1. **Project(s):** which project(s) this chat was about. A chat can touch more than one; handle each. If the chat was general and no real work happened, log tasks with no project.
2. **Done:** concrete things completed in this chat (built, written, decided, sent, fixed). One task per meaningful outcome, not per message. Name them as outcomes ("Wrote PRD v1", "Fixed login redirect bug").
3. **Pending:** what is still left: next steps suggested that the user didn't do yet, open questions, blockers, things the user said they'd do later. Keep any dates, people or priorities mentioned.
4. **Reminders and follow-ups:** anything time-bound ("remind me", "follow up with X on Thursday", deadlines). These get `Type` = Reminder or Follow-up and a `Due` date.
5. **Docs:** substantial written output worth keeping: specs, plans, PRDs, research, drafts, meeting notes, decisions, code explanations, links to files or artifacts. Short chat answers are not docs.
6. **Progress:** an honest estimate of how far each project is (0–100%) and its single most important next step.

Resolve "tomorrow", "Friday" etc. against today's date in the user's time zone.

## Step 2: Find or create the project

- Look in Projects for the project by name, including close variants and abbreviations. To look up records, use `notion-query-data-sources` in `rows` mode with a title filter; if that's unavailable or hits a plan limit, use `notion-search` and fetch the results.
- **Existing project:** update `Status` (Idea → Active once work starts; Done only if the user said it is finished), `Progress`, `Next step`, and `Deadline` if one was set. Never lower Progress unless the scope clearly grew, and say so in the log if you do.
- **New project:** create it with `Project`, `Status` (Active if work was done, else Idea), `Area` (best guess: work for a client → Client; own app, startup or product → Product; jobs, CV, interviews → Career; courses or practice → Learning; else Personal), `Progress`, `Next step`, `Owner`. Put a 2–3 line description of the project at the top of its page.
- **Ambiguous** (two existing projects could match, or it's unclear whether it's new): ask one short question listing the candidates, then continue.

## Step 3: Tasks (no duplicates)

1. Look up existing tasks linked to that project, and recent unlinked tasks with similar titles.
2. For each **done** item: if a matching open task exists, set its `Status` to Done. Otherwise create it with `Status` = Done, linked to the project, with `Owner`.
3. For each **pending** item: if a matching open task exists, update it (due date, priority) instead of creating a copy. Otherwise create it with:
   - `Status` = To do (or Doing if it's actively in progress)
   - `Priority` = High if blocking or due within 2 days, else Medium unless stated
   - `Due` only if a date was mentioned
   - `Type` = Task, Reminder or Follow-up
   - `Project` relation and `Owner` (the user, or a teammate if the chat assigned it to someone: look them up by name; if not found, put their name in the task title and keep the user as Owner)
4. Keep titles short and action-first. Add detail in the task's page body only if needed.

## Step 4: Docs

For each doc identified:

- Look in Docs & Notes for an existing doc with the same purpose for this project. If found, update it (add a dated section or replace outdated parts) instead of making a new one.
- Otherwise create a page in Docs & Notes with `Title`, `Type`, the `Project` relation, and the full content in clean Notion markdown (headings, lists, tables, code blocks). Keep the substance of what was produced in the chat: don't shrink a spec into three bullets. If the output was a file or artifact, include its name and link.

Always also create one **session log** in Docs & Notes:

- `Title`: `Log: <project name>, <DD Mon YYYY>`
- `Type`: Note, with the `Project` relation set
- Body:
  - **Summary:** 2–4 sentences on what this chat accomplished
  - **Done:** bullet list, mentioning each task page
  - **Pending:** bullet list with due dates, mentioning each task page
  - **Decisions:** key choices and why
  - **Docs:** mentions of docs created or updated
  - **Progress:** old % → new %, and the next step

Use Notion page mentions (`<mention url="PAGE_URL" />`) for every task and doc so it's all clickable.

## Step 5: Link the project page

The Tasks and Docs relations already show on the project. Also add a dated one-line entry to the project page under a `## Progress log` heading (create the heading if missing): `<date>: <one-line summary> (mention of the session log)`. This builds a running history.

## Step 6: Report back

Reply in the chat, briefly:

- **Project:** name, new or existing, Progress old → new, Next step
- **Tasks:** X done, Y pending (list the pending ones with due dates)
- **Docs:** created or updated (titles)
- Anything you were unsure about or skipped

Keep it short. The user can open Work Home for the rest.

## Rules

- Never delete, archive or trash anything, and never rename or remove the user's database properties.
- Never invent work that didn't happen or dates nobody mentioned.
- Prefer updating over creating whenever a matching record exists.
- Only write inside the three databases and the Work Home page (plus project pages' bodies). Don't edit other pages in the user's workspace.
- If the user adds notes after `/log-notion` (e.g. "/log-notion mark the landing page as done, rest due Friday"), treat those notes as corrections that override what you inferred.
