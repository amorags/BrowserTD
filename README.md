# Towerbloom

A small tower-defense game built by hand while refreshing TypeScript, functions, state, and data flow.

The first target is one straight lane, one tower type, a placement choice, three short waves, lives, a win/lose result, and restart. The player-facing title currently says **Elemental Tower Defense**; the final name is open.

## Continue the project

1. Read [Current progress](docs/PROGRESS.md) for the last verified state and exact next action.
2. Follow the checkboxes in the relevant milestone of the [Detailed roadmap](docs/ROADMAP.md).
3. Work in small pieces: add code, run it, change something, and explain the result.
4. Update progress and check off only the work actually completed.

[AGENTS.md](AGENTS.md) records the pair-programming agreement for future assistant sessions.

## Start the existing development server

From the project directory:

```sh
npm run dev
```

The script is already configured. Browser startup has not been verified as part of preparing this documentation.

## Work on two PCs with Git

Clone the GitHub repository once on the other PC, then open a terminal in that clone. Both PCs need Node.js and npm installed.

At the start of a session, with your previous changes committed:

```sh
git status
git pull --ff-only
npm ci
npm run dev
```

`npm ci` installs the dependency versions recorded in `package-lock.json`. Run it after the first clone and whenever the lockfile changes; it is optional when dependencies are already up to date.

Before switching PCs, stop the dev server with Ctrl+C if it is running in this terminal, then save your work:

```sh
git status
git add .
git diff --cached
git commit -m "Describe the change you made"
git push
```

`git add` stages the changes for the next commit. `git diff --cached` lets you review that snapshot. A commit records it locally; pushing copies the commit to GitHub. Include progress-document updates so the next session knows where to resume.

Keep `package-lock.json` in Git. The root `.gitignore` excludes `node_modules`, build output, logs, and local environment files; each PC installs its own dependencies.

If `git pull --ff-only` reports that branches have diverged, inspect the history and resolve the difference before continuing. The intended routine is to push before leaving one PC and pull before editing on the other.

## Resume in a new chat

Paste this when starting a chat in this project:

> Continue our Towerbloom pair-programming project. Read AGENTS.md, docs/PROGRESS.md, and the current milestone in docs/ROADMAP.md. Inspect the relevant current files before suggesting edits. Tell me the next small action and guide me through it with small snippets and exact insertion points. I will edit the game code unless I explicitly ask you to do so. Preserve my design choices, and help me keep the progress documents current.

The roadmap is a sequence of working increments, not a deadline. A milestone may take several sessions, and design choices can change as we try the game.
