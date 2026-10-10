# Current progress

Last updated: **2026-10-10** (Europe/Copenhagen).

## Resume here

- **Current milestone:** M0, confirm the existing shell; then M1, draw the first scene.
- **Last completed work:** the learning roadmap and session handoff documents have been created.
- **Game implementation progress:** an HTML page and development command exist; `src/main.ts` is empty.
- **Exact next action:** ask the user to run `npm run dev` and confirm the title and empty canvas appear. If they have already verified this, record that and start M1.1: obtain the canvas in `src/main.ts` and explain the lookup/check.
- **Files for the next step:** `package.json`, `index.html`, `src/main.ts`.
- **Do not skip ahead:** getting the canvas and drawing one shape is enough for the first code exchange.

## Verified baseline

These facts were observed by reading files on 2026-10-10:

- `package.json` defines `dev` as `vite --open`.
- Vite, TypeScript, Biome, and Vitest are listed as development dependencies. A `node_modules` directory exists; dependency health has not been verified.
- `index.html` contains a heading, instruction text, and a canvas with ID `game`, width `800`, and height `600`.
- The canvas has a green border. The page and heading currently use the title `Elemental Tower Defense`.
- The HTML loads `/src/main.ts` as a module.
- `src/main.ts` exists and is empty.
- `biome.json` exists. Formatting and linting have not been run for this handoff.
- No project `tsconfig.json` was present. The current `test` script is the initial placeholder that exits with an error.
- The instruction text uses `<p1>`, which should become a normal `<p>` during M0.
- The existing HTML describes mouse placement, but that behaviour has not been implemented yet.
- No development server, game behaviour, type check, or test suite was run while preparing this documentation.
- Game source and configuration were not changed while preparing this documentation.

## Design and learning decisions

| Decision | Status | Reason / revisit point |
| --- | --- | --- |
| TypeScript, Canvas 2D, Vite; plain functions and objects | Agreed direction | Matches the current project and learning goal. |
| Small snippets, exact edit locations, user writes game files | Agreed workflow | Practice syntax and ownership while keeping momentum. |
| Drawing and movement before economy and upgrades | Agreed priority | Reach visible behaviour early. |
| Straight lane, one tower type, moving waves | Agreed first foundation | Expand existing functions as needs appear. |
| First level has three short waves and lives | Proposed starting scope | Tune the counts after the first combat prototype. |
| Choose one of three build spots; one active tower | Proposed interaction | Revisit at M13; free placement remains an option. |
| Immediate damage with a brief beam/flash | Proposed first weapon | Revisit combat feel at M10. Moving projectiles can follow. |
| Fixed ticks, simulation separated from browser rendering | Planned at M6 | Supports understandable rules and later testing. |
| Gold, income, upgrades, speed controls, maze building | Deferred | Revisit after the first small level. |

## Open questions

- Has the user already successfully run the current page in the browser?
- What lane position, colours, and shape sizes feel good after M1?
- How much explanation of TypeScript syntax is useful once coding starts?

Later design questions belong at their roadmap checkpoints; they do not block the first drawing step.

## Session log

### 2026-10-10 — Plan and continuity setup

- **Work:** translated the agreed pair-programming path into `docs/ROADMAP.md`; added this handoff, a README, and persistent assistant instructions.
- **Evidence:** inspected existing HTML, package scripts, Biome configuration, and empty TypeScript entry file.
- **Learning/design:** start with supplied small examples and experiments; increase independent implementation as familiarity returns.
- **Verification limit:** files inspected only; runtime behaviour remains unverified.
- **Next:** confirm the current page starts, then acquire the canvas and draw the first scene.

### Copy this block for the next session

Date and short title:

- **Milestone / substeps:**
- **Files changed:**
- **Observed result:**
- **Checks actually run / user-reported checks:**
- **Concepts practiced / explanation still needed:**
- **Experiment, prediction, and result:**
- **Design choices made:**
- **Unfinished work / blockers:**
- **Exact next action:**

## Small bug and structure journal

Add a short entry when a real issue teaches us something. No need to invent entries to fill a quota.

| Date | Symptom | Cause | Change / lesson |
| --- | --- | --- | --- |
| — | No entries yet | — | — |
