# Code review add-on for the workspace (Codex)

**What this is.** An add-on to the workspace starter prompt. It adds one step to `/checkpoint`: when code was changed in the session, a second AI from a different vendor (OpenAI's Codex) reviews it before the session is saved. Sessions that only touch notes and documents are not affected.

**Before you paste it.**

1. **An OpenAI account that includes Codex, approved for this use.** The review sends the changed code to OpenAI. If any project holds code that must not leave the computer, say so when Claude asks; that project is then never sent.
2. **The Codex plugin installed in Claude Code.** Type these one at a time:
   - `/plugin marketplace add openai/codex-plugin-cc`
   - `/plugin install codex@openai-codex`
   - `/codex:setup`

   Setup checks for the Codex tool and offers to install it if it is missing (this needs Node.js on the computer). If it says you are not signed in, type `!codex login` and follow the sign-in. On a Team or Enterprise account an owner may have to allow the plugin source first.

   The first command downloads the plugin from a public GitHub page. That needs an internet connection but no GitHub account, and nothing of yours is sent to GitHub. The workspace's own history stays on this computer.
3. **Start a new session in the workspace folder** after installing, then paste everything below the line. Claude will ask permission to edit its `.claude` folder. Say yes.

**What it is and is not.** It is a first pass that catches mistakes before they are saved. It is not a control: the session that wrote the code is the one that calls the review, and a save still goes through when the reviewer cannot be reached. While the history is kept only on this computer, this is the only review there is, so someone should read the "not reviewed" lines in the session notes and decide what to do about them. If the code later moves to a shared code host and the firm relies on AI review as its independent check on changes, that check belongs in the host's settings, where a change cannot be accepted without it.

**Keep it current.** Nothing updates the Codex tool by itself. Run `/codex:setup` now and then, and before any large review.

---

Add a code review step to this workspace's `/checkpoint` command. Do exactly what is described here. If something does not fit what you find, ask before changing anything.

## Step 1: check that everything is in place

1. Check this is the workspace root: it holds `CLAUDE.md`, `projects/` and `.claude/skills/checkpoint/SKILL.md`. If it does not, say so and stop.
2. Check the Codex plugin is installed: an agent named `codex-rescue` (it may be listed as `codex:codex-rescue`) must be available to you. If it is not, tell me to install the plugin and start a new session, then stop. Do not install anything yourself and do not change any file.
3. Ask me one question and wait for the answer: is there any project whose code must never be sent to an outside reviewer?

## Step 2: add the step to the checkpoint command

Edit `.claude/skills/checkpoint/SKILL.md`. Writing into `.claude/` needs my approval and Claude Code will ask me for it. If the write is refused, do not work around it; tell me and ask me to approve it.

Make these four changes and no others. If the file has been changed since it was first set up, keep those changes.

**a.** In the `description` line at the top, after "Save the session.", add: "Reviews any changed code with Codex first."

**b.** Insert the following as a new step straight after the step that works out which projects the session touched, and renumber the steps after it.

````markdown
3. Code review. Do this before writing any notes.
   - List what changed since the last commit, new files included: `git status --short --untracked-files=all`. Code means files that run: scripts, programs, queries, notebooks, and settings files that change how they run. Notes, documents, data files and everything under `.claude/` and `memory/` are not code. If no code changed, put "No code changed, no review." in the final reply and go to the next step.
   - Leave out any project whose `CLAUDE.md` says its code is not sent to an outside reviewer, and say which projects were left out.
   - Before anything is sent, look through the changed code for passwords, keys, tokens, login details and anything `CLAUDE.md` says must never be written here. If you find any, stop and tell the user. Send nothing.
   - Hand the review to the `codex-rescue` agent from the Codex plugin. Tell it: this is a review only and it must change no files; name the files to review, as they stand against the last commit; look for bugs, security problems and failures that would pass without anyone noticing; give each finding a severity (critical, high, medium or low), the file and line, and one sentence on what goes wrong.
   - Critical or high findings: show them to the user and fix each one, then say what you changed. If a fix would change what the code is meant to do, or you think the finding is wrong, stop and ask the user instead of fixing. Do not send the fixed code for a second review.
   - Medium or low findings: list them. Change nothing unless the user asks.
   - If the agent is missing, not signed in, fails or returns nothing usable, try once more at most. Then record "Code not reviewed:" with the reason and carry on with the checkpoint. A review that could not run never blocks a save, and is never recorded as a review.
   - Carry the result into the session entry for each project whose code was involved: reviewed or not reviewed, by what, how many findings at each severity, and what was fixed.
````

**c.** In the step that writes each project's session entry, add to the list of what the entry holds: "and the code review result".

**d.** In the last step, the reply, add: "Give the code review result in one line: reviewed with the count of findings, not reviewed with the reason, or no code changed."

## Step 3: record it in the instruction files

1. In the root `CLAUDE.md`, under the "Checkpoint" heading, add:

   ````markdown
   When code was changed in the session, checkpoint sends the changed code to Codex (OpenAI) for review before saving. A project whose `CLAUDE.md` says its code is not sent to an outside reviewer is never sent. The review is a first pass, not a sign-off.
   ````

2. For each project I named in answer to your question, add this line under "Rules for this project" in that project's `CLAUDE.md`:

   ````markdown
   - Code in this project is not sent to an outside reviewer.
   ````

## Step 4: show me

1. Show me the whole of the changed checkpoint file, so I can see the new step and the renumbering.
2. List every file you changed, then commit them now, adding them by name, with the message "Add code review to checkpoint". Do not leave them for the next checkpoint: a later session will not treat them as its own changes and will leave them out.
3. Tell me in two lines what will now happen at checkpoint when code has changed, and what happens when the reviewer cannot be reached.
