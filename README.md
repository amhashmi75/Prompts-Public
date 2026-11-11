# Claude Code workspace starter

Two prompts for running several projects from one Claude Code session, with notes and memory that survive between sessions.

Written by Aijaz Hashmi. First published November 2026.

## What is here

| File | What it does |
|---|---|
| `workspace-starter-prompt.md` | Sets up one root folder with a folder and instruction file per project, a one-page status for each, session notes, a decisions log, a memory folder, and three commands: `/session-resume`, `/checkpoint` and `/new-project`. |
| `workspace-code-review-add-on.md` | Optional. Adds a step to `/checkpoint` so that a second AI from a different vendor (OpenAI's Codex) reviews any code changed in the session before it is saved. |

## How to use it

1. Make or choose a folder that will hold all your projects, and open Claude Code in it.
2. Open `workspace-starter-prompt.md` and paste everything below the line as your first message.
3. Answer the four questions. Claude builds the workspace and shows you what it made.
4. From then on, start Claude Code in that same folder, say which project to work on, and say "checkpoint" before closing.

Each file carries its own notes on what to expect, including the notes for the Claude desktop app.

## The ideas behind it

- One page for status, rewritten at every checkpoint, so it keeps being read.
- What happened, what was decided and what stays true are kept in three different places.
- Notes can be wrong: check the thing itself before saying how it stands now.
- A correction is saved at once, before the work carries on.
- Files are added to the history by name, never all at once.
- Memory lives inside the folder, where it is visible, in the history and backed up.

## What has and has not been tested

- The starter prompt was run twice from an empty folder in the terminal version of Claude Code. The build, a checkpoint after real work, a saved correction, a new project and a resume from a cold start all worked.
- The add-on was run on three paths: pasted with no plugin installed, a session with no code changed, and a session with code changed and no reviewer available.
- Not tested: the Claude desktop app, and a live review by Codex.

It keeps history with git on your own computer. It does not need GitHub or any other outside service.

## Licence

MIT. See `LICENSE`.
