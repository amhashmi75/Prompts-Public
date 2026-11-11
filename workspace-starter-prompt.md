# Workspace starter prompt for Claude Code

Copyright (c) 2026 Aijaz Hashmi. Released under the MIT licence; see `LICENSE`.

**How to use this.** Make or choose a folder that will hold all your projects. Open Claude Code in that folder. Paste everything below the line as your first message. Claude asks four questions, then builds the workspace and shows you what it made. From then on, always start Claude Code in that same folder.

**One thing to expect.** Claude Code guards its own settings folder (`.claude/`), which is where the three commands live. It will stop and ask your permission before writing them. Say yes; the choice that allows edits to this project's `.claude` folder for the session saves being asked three times. Run this in an ordinary session where you can answer, not unattended.

**In the Claude desktop app (Code tab).**

- Choose a **Local** session on the folder. A Cloud session works on a copy of a GitHub project, so nothing would be written to your folder.
- Leave **Worktree** switched off, which is the default. Switched on, each session works in a side copy and your notes do not reach the main folder until they are merged.
- Git must be installed on the computer.
- If the three commands do not appear when you type `/` straight after the build, type `/reload-skills` or start a new session.

It sets up three things: a folder per project with its own instruction file, one place for notes and memory that survives between sessions, and three commands (`/session-resume`, `/checkpoint`, `/new-project`).

---

You are setting up this folder as a workspace. One main Claude Code session, always started in this folder, will manage several separate projects that live in folders under it. Each project has its own instruction file and its own notes. Everything you need to remember between sessions is kept as plain files in this folder, so it is visible, kept in the version history and backed up with the folder.

Build exactly what is described here. Where something is unclear or does not fit what you find, ask before you build.

## Step 1: ask me these four questions, all together, and wait for my answers

1. **Who you are working with.** My name, my role, how I want you to write to me (plain English or technical, brief or detailed), and a name for this workspace.
2. **The first projects.** A name and one sentence for each. Two to five is a good start; more can be added later with `/new-project`.
3. **Version history.** The workspace keeps its history with git on this machine. Should it also copy to an outside service such as GitHub? If I do not say yes and name the place, the answer is no, and you never add one later without being told.
4. **Limits.** Is there any kind of material that must never be written into these files (for example client data or personal data)? Should any project be kept apart from the others, so that nothing is carried between them?

If this folder is not empty, list what is in it and propose where each thing would go. Move, rename or delete nothing until I agree.

## Step 2: build this layout

```
<this folder>/
  CLAUDE.md                     workspace instructions, read at the start of every session
  STATUS.md                     the whole workspace on one page
  .gitignore
  memory/
    MEMORY.md                   index, one line per note
    HOW-TO.md                   the form a memory note takes
  notes/
    sessions.md                 record of work that belongs to no single project
  projects/
    <project-name>/             one folder per project, lowercase words joined by hyphens
      CLAUDE.md                 what this project is and its own rules
      STATUS.md                 where this project stands, one page
      notes/
        sessions.md             what happened in each session, newest first
        decisions.md            decisions and the reasons for them
      work/                     the project's own files
  .claude/
    skills/
      session-resume/SKILL.md
      checkpoint/SKILL.md
      new-project/SKILL.md
```

If the folder is not already a git repository, run `git init` in it. Do not add an outside copy unless my answer to question 3 named one. If git has no name and email set for commits on this machine, ask me what to use.

Writing into `.claude/` needs my approval, and Claude Code will ask me for it. If that write is refused, do not skip the skill files and do not work around the block. Tell me, and ask me to approve it.

## Step 3: write these files

Fill each `<...>` from my answers. Keep the wording of the rules as written.

### `CLAUDE.md` (workspace root)

Keep this file short. It is read in full at the start of every session, so detail belongs in the project files.

````markdown
# <Workspace name>

One main Claude Code session, started in this folder, manages every project under `projects/`. This folder is the workspace root.

## Who I am working with

<Name, role, and how they want to be written to.>

## Session start

At the start of a new session run `/session-resume` before anything else. If my first message is already a specific task, read only what that task needs and get on with it.

## Projects

| Project | Folder | What it is |
|---|---|---|
| <name> | `projects/<folder>/` | <one sentence> |

Detail about a project lives in that project's own `CLAUDE.md`, not here.

## Working on a project

- Before working on a project, read its `CLAUDE.md` and its `STATUS.md`. Where its rules differ from this file, its rules win.
- Say which project you are working in whenever that changes.
- Keep each project's files inside its own folder. Do not carry material from one project into another unless I ask.
- <Kept apart: nothing moves between project A and project B. If I ask for something that would cross, check with me first that I mean it. Delete this line if no projects are kept apart.>
- Work that belongs to no project is recorded in the root `notes/`. If it keeps growing, propose a new project.

## Where things are kept

| What | Where |
|---|---|
| The whole workspace at a glance | `STATUS.md` |
| Where one project stands now | `projects/<name>/STATUS.md` |
| What happened in each session | `projects/<name>/notes/sessions.md` |
| Decisions and the reasons | `projects/<name>/notes/decisions.md` |
| Standing rules for one project | `projects/<name>/CLAUDE.md` |
| Lasting facts about me and how I want you to work | `memory/` |

Memory for this workspace lives in `memory/` in this folder. Save it here and nowhere outside this folder. Claude Code's built-in memory keeps its notes elsewhere on this computer, outside the history and the backup, so do not rely on it for anything that matters.

## Memory rules

- One fact per file in `memory/`, in the form given in `memory/HOW-TO.md`, with one line for it in `memory/MEMORY.md`. Before adding a note, look for one that already covers it and update that instead.
- Save a note when I correct you (save it before carrying on), when I state a standing preference, when you learn where something outside this folder is kept, or when you learn a fact you would otherwise have to be told again.
- Do not save a record of work done (that belongs in the session notes), anything the files already say, or anything that only matters today.
- Write dates in full, never "yesterday" or "next week".
- Notes say what should be true. Before you state how a file, a system or a piece of work stands now, check it. If you cannot check, say "the notes say".
- Correct or delete a note that turns out to be wrong.

## Never write into any file here

- Passwords, keys, tokens or login details. Name the place they are kept instead.
- <Material named in answer 4, or delete this line.>

## Version history

- History is kept with git in this folder. <"There is no outside copy and none is to be added." or the agreed place.>
- Add files by name. Never add everything at once.
- Commit at each checkpoint, not in between, unless I ask.

## Checkpoint

When I say "checkpoint", "save" or "wrap up", run `/checkpoint`.

## How to work

- State any assumption that is not obvious. If a request can be read two ways, show me both before choosing.
- Keep each change to what was asked. If you notice something else, tell me and leave it alone.
- Say what you checked and what you did not. Report a failure plainly.
- Before creating a file that will be produced again and again, say where it will live and when old copies get cleared.
````

### `STATUS.md` (workspace root)

````markdown
# Workspace status

Updated <date>. One page. Detail is in each project's own `STATUS.md`.

| Project | Where it stands | Next step | Waiting on |
|---|---|---|---|
| <name> | Set up on <date>, no work yet | | |

## Across projects

- <Anything that touches more than one project, or "Nothing yet.">
````

### `projects/<name>/CLAUDE.md` (one per project)

````markdown
# <Project name>

<The one sentence I gave, and anything else I said about it.>

Where these instructions differ from the workspace `CLAUDE.md`, these win.

## What this project is for

<Purpose, who it is for, what a finished result looks like. Ask me if you do not know.>

## Rules for this project

- <Rules that apply only here. "None yet." is a fine start.>

## Where things are

- `STATUS.md`: where the project stands, one page.
- `notes/sessions.md`: what happened in each session, newest first.
- `notes/decisions.md`: decisions and the reasons.
- `work/`: the project's own files.
````

### `projects/<name>/STATUS.md`

````markdown
# <Project name>: status

Updated <date>. One page.

## Where we are

- Set up on <date>. No work yet.

## Next

1.

## Open

- <Questions not yet answered, and anything waiting on someone, with who.>
````

### `projects/<name>/notes/sessions.md`, and the root `notes/sessions.md`

````markdown
# Sessions

Newest first. One dated entry per session.
````

### `projects/<name>/notes/decisions.md`

````markdown
# Decisions

Newest first. Each entry: the date, what was decided, why, and who decided.
````

### `memory/MEMORY.md`

````markdown
# Memory index

One line per note: `- [Title](file.md): what it is, in a few words`. The facts themselves are in the files, never here.

## Notes

- <None yet.>
````

### `memory/HOW-TO.md`

Write this file as well, so that a later session knows the form of a note without being told.

````markdown
# How a memory note is written

Each note is its own file in `memory/`, named after its `name`, in this form:

```
---
name: <short-name-with-hyphens>
description: <one line saying what this is, used to decide whether to read it>
type: <user | feedback | project | reference>
applies-to: <all, or a project folder name>
---

<The fact. For feedback, add a line saying why and a line saying when it applies.>
```

- `user`: who I am and how I work.
- `feedback`: a correction, or a way of working I confirmed.
- `project`: a lasting fact about a project that its own files do not record.
- `reference`: where something outside this folder is kept.

After writing a note, add its one line to `MEMORY.md`.
````

### `.gitignore`

```
.env
.env.*
*.key
*.pem
.DS_Store
Thumbs.db
~$*
.claude/worktrees/
```

### `.claude/skills/session-resume/SKILL.md`

````markdown
---
name: session-resume
description: Recover where things stand at the start of a session and give a short briefing. Use when the user says "resume", "where were we", "catch me up" or "what's the status", and at the very start of a new session when no specific task has been given. An optional project name narrows it to that project.
argument-hint: "[project]"
---

# Session resume

Give a short briefing on where things stand. Read only what the steps name, not whole histories.

Project named by the user, if any: $ARGUMENTS

1. Run `date` and `pwd`. Check that this folder holds `CLAUDE.md` and `projects/`. If it does not, say the session was not started in the workspace root and stop.
2. Read `STATUS.md`, `memory/MEMORY.md` and the newest entry in `notes/sessions.md`.
3. If a project was named: read its `CLAUDE.md`, its `STATUS.md`, the two newest entries in its `notes/sessions.md`, and any memory note whose `applies-to` is that project or `all` and whose description bears on the work.
   If no project was named: read each project's `STATUS.md`. With more than six projects, read only those whose row in the root `STATUS.md` shows a next step or something waiting.
4. Check for work that was never saved: `git status --short` and `git log --oneline -5`. Changed files with no checkpoint after them mean the last session ended without saving. Say so and name the files.
5. Give the briefing, twelve lines at most:
   - today's date;
   - one line per project: where it stands and the next step;
   - what is waiting on the user or on someone else;
   - any unsaved work found in step 4;
   - the single most useful thing to do next.

Say "the notes say" for anything you read and did not check. Do not start work until the user says which thing to pick up.
````

### `.claude/skills/checkpoint/SKILL.md`

````markdown
---
name: checkpoint
description: Save the session. Updates status pages, session notes, decisions and memory, then commits. Use when the user says "checkpoint", "save", "save progress" or "wrap up", and at the end of any long piece of work.
---

# Checkpoint

Record this session so that a new session can pick it up cold.

1. Run `date` and `pwd`, and check this is the workspace root (it holds `CLAUDE.md` and `projects/`).
2. Work out which projects this session touched, from the conversation and from `git status --short`. If nothing changed and nothing worth keeping was said, reply "Nothing to checkpoint." and stop.
3. For each project touched:
   - Rewrite its `STATUS.md` so it is true now. Keep it to one page: take out what is finished, do not just add to it.
   - Add a dated entry at the top of its `notes/sessions.md`: what was asked, what was done, what was decided, what is left, and what is waiting on whom. Write it for a reader who was not here.
   - Add each decision to its `notes/decisions.md`: the date, what was decided, why, and who decided.
4. Update that project's row in the root `STATUS.md`. Work that belonged to no project gets a dated entry at the top of the root `notes/sessions.md`.
5. Memory. Save lasting facts from this session under the memory rules in `CLAUDE.md` and in the form in `memory/HOW-TO.md`: corrections the user made, standing preferences, where outside things are kept. One fact per file, one line in `memory/MEMORY.md`. Update an existing note in preference to adding one. Do not save a record of the work.
6. Housekeeping. If a `sessions.md` has passed about 300 lines, move its oldest entries, word for word, to `notes/archive/sessions-<year>.md` beside it. If a `STATUS.md` has grown past a page, cut it back.
7. Look through every file you are about to commit for passwords, keys, tokens, login details and anything `CLAUDE.md` says must never be written here. If you find any, stop and tell the user. Do not commit.
8. Commit. Add files by name, never everything at once. Use one commit with a message that starts "Checkpoint:" and says in a line what the session did. Send it to an outside copy only if `CLAUDE.md` names one.
9. Reply "Checkpoint saved." with one line per project on what was recorded, and say plainly if anything was left unsaved and why.
````

### `.claude/skills/new-project/SKILL.md`

````markdown
---
name: new-project
description: Add a new project folder to the workspace with its own instruction file, status page and notes. Use when the user asks to add, start or set up a new project.
argument-hint: "[project name]"
---

# New project

Name given by the user, if any: $ARGUMENTS

1. If you do not have them, ask for the project's name and one sentence on what it is. Ask whether it must be kept apart from any other project.
2. Choose a folder name: lowercase words joined by hyphens. If `projects/<folder>/` already exists, say so and stop.
3. Create `projects/<folder>/` with `CLAUDE.md`, `STATUS.md`, `notes/sessions.md`, `notes/decisions.md` and an empty `work/` folder holding a `.gitkeep` file. Copy the form of an existing project's files, filled in for this one.
4. Add the project to the table in the root `CLAUDE.md` and to the table in the root `STATUS.md`. If it must be kept apart, add that to "Working on a project" in the root `CLAUDE.md`.
5. Show the user what you created. It is committed at the next checkpoint.
````

## Step 4: check your work and show me

1. List the full tree of what you built.
2. Put an empty `.gitkeep` file in each project's `work/` folder so the folder is kept in the history.
3. Read each skill file back and confirm it starts with the `---` block exactly as written above.
4. Make the first commit, adding the files by name, with the message "Workspace set up".
5. Follow the session-resume steps once and show me the briefing, so I can see it working.
6. Tell me anything you changed from this description and why, and anything you could not do.

Then tell me, in three lines, how to use it: start Claude Code in this folder, say which project to work on, and say "checkpoint" before closing. Add that if the three commands do not appear when I type `/`, I should type `/reload-skills` or start a new session.

## Why it is built this way

These rules come from running a setup like this for most of a year. Keep them in mind when something here seems fussy.

- **One page for status.** A status file that only ever grows stops being read. It is rewritten at every checkpoint, and history goes to the session notes.
- **Sessions, decisions and memory are three things.** What happened, what was decided, and what stays true are looked up in different ways. Mixed in one file, none of them can be found.
- **Notes can be wrong.** A note records what was true when it was written. Check the thing itself before saying how it stands now.
- **Corrections are saved at once.** A fact the user has to correct twice is the commonest way a setup like this loses their trust.
- **Files are added by name.** Adding everything at once is how a password file or a large data file ends up in the history, where it stays.
- **Memory stays in this folder.** Notes kept elsewhere on the machine are not in the history, not in the backup, and not visible to the person who owns the work.
