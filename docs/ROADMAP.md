# Towerbloom: from an empty canvas to a small playable level

## Purpose and working agreement

Build a game while reconnecting existing programming knowledge with TypeScript syntax, functions, data passing, and state. Each increment should have a visible or inspectable result and leave something concrete to discuss.

This roadmap develops the tower-defense portion of the user's *Game & Tech Plan: Oct 2026 – Jun 2027* (`kafkabythenorwegianwood.pdf`). The user's newer request sets the learning format: guided code examples, hands-on placement of snippets, experiments, questions, and gradually more independent work. The broader portfolio plan stays in the background.

The assistant provides a small change with its exact location. The user edits, runs it, and experiments. Typical chunks are a few lines or one function; a larger step is split across exchanges. Existing functions grow as new behaviour needs them. File moves happen separately from behaviour changes whenever practical.

Read [PROGRESS.md](PROGRESS.md) before resuming. It records the current state and exact next action. This document owns the checkboxes. An unchecked box means there is still work or verification to do, even if an example has already been provided in chat.

## What counts as the first completed level

The proposed target is:

- One straight lane with a clearly marked entrance and exit.
- One tower type and one enemy type, drawn with simple shapes.
- One tower placed at a choice of three build spots before the level starts.
- Three short waves, with a start action between waves.
- Automatic targeting, visible attacks, enemy health, and lives lost to escapes.
- Victory after the final wave is resolved, defeat at zero lives, and a working restart.
- Balance values that can be changed without hunting through drawing code.

Placement rules, wave count, and weapon presentation are proposed defaults. Choose and record changes when their checkpoints arrive. Gold, income, upgrades, speed settings, branching paths, and maze building are later features.

## How to use each milestone

1. Read its visible goal and choose the first unfinished substep.
2. Add the smallest change that can be observed or explained.
3. Check the expected result before stacking another feature on top.
4. Try an experiment. Predict, run, observe, and discuss the difference.
5. Tick completed tasks and record important decisions in the progress page.
6. Finish with an exact next action that another chat can pick up.

The reflection prompts are conversation starters. Familiar concepts can move quickly; confusing concepts get a concrete example or a smaller step. No milestone has a mandatory duration.

## Route at a glance

| Milestones | Visible result | Main practice |
| --- | --- | --- |
| M0–M3 | Static lane, enemy, and tower represented as data | Browser access, drawing, functions, objects, types |
| M4–M6 | One moving enemy and a small separated simulation | Time, returned state, modules, update/render flow |
| M7–M10 | A tower fights a finite wave | Collections, spawning, IDs, targeting, combat |
| M11–M12 | Readable action and a complete automatic round | Events, lifecycles, win/lose, reset |
| M13–M15 | A player can place, play, finish, and retry a small level | Input, commands, level data, tuning, focused tests |

## M0 — Confirm the existing shell

**Visible goal:** the current heading and empty canvas open in the browser.

**Files:** existing `package.json`, `index.html`, and `src/main.ts`. No new game modules.

- [x] M0.1 Confirm a `dev` script exists: `vite --open`.
- [x] M0.2 Confirm the HTML has the canvas and a module script referencing `/src/main.ts`.
- [x] M0.3 Confirm the TypeScript entry file exists and is empty.
- [ ] M0.4 Run `npm run dev`; confirm the page opens and the canvas border is visible.
- [ ] M0.5 Change the instruction paragraph's `<p1>` tags to `<p>` and confirm the page still displays correctly.
- [ ] M0.6 Open the browser console and identify where runtime errors would appear.

**Understand:** Node runs the development tooling; the game code runs in the browser. The HTML loads the TypeScript entry through Vite. The package's `main` field is not the browser entry.

**Experiment:** change the heading or border colour, save, and observe the result.

**Ready to continue when:** the existing page is confirmed working. Record whether this was observed by the assistant or reported by the user.

## M1 — Draw the first scene

**Visible goal:** one horizontal lane, one enemy circle, and one tower square.

**Files:** `src/main.ts` only. Keep the first drawing instructions together.

- [ ] M1.1 Look up the canvas by its existing `game` ID; check/narrow the element before using it.
- [ ] M1.2 Obtain the 2D drawing context and handle the possibility that it is unavailable.
- [ ] M1.3 Draw one simple shape first and confirm the connection works.
- [ ] M1.4 Draw the horizontal path, then add the enemy and tower shapes one at a time.
- [ ] M1.5 Give the important positions, sizes, and colours readable names.
- [ ] M1.6 Change at least two values deliberately and explain the results.
- [ ] M1.7 Save a screenshot of the first scene and note where it was saved.

**Understand:** method calls, arguments, coordinates, and the canvas drawing context. Explain `const`, member access, and any type-narrowing syntax when it appears.

**Experiment:** move the path down, double the enemy radius, and move the tower above the lane. Try predicting which direction increasing `y` moves a shape.

**Check:** all shapes are visible; no console errors; the user can find the number controlling each position.

**Design checkpoint:** choose initial colours, lane position, and shape sizes. These remain easy to change.

## M2 — Give repeated drawing work a name

**Visible goal:** the same scene is drawn by small functions.

**Files:** `src/main.ts`. Introduce `drawPath`, `drawEnemy`, `drawTower`, and a coordinating `render` gradually.

- [ ] M2.1 Move the enemy drawing instructions into `drawEnemy` and call it once.
- [ ] M2.2 Pass position and size into the function; identify parameters and arguments.
- [ ] M2.3 Draw a second enemy by calling the same function with different values.
- [ ] M2.4 Extract the path and tower drawing functions one at a time.
- [ ] M2.5 Add `render` to call the drawing functions in an intentional order.
- [ ] M2.6 Remove the temporary second enemy if it is no longer needed for the scene.

**Understand:** declaration versus call, scope, parameters, and side effects. These drawing functions affect the canvas; they do not need to return an enemy.

**Experiment:** reverse two drawing calls where shapes overlap and observe which appears on top.

**Check:** extracting functions leaves the original scene intact; a single function can draw at two different positions.

**Reflection:** trace one coordinate from a call into the function and finally into a canvas method.

## M3 — Describe the scene with objects

**Visible goal:** the same shapes are drawn from enemy and tower data.

**Files:** `src/main.ts`. Introduce only the types and fields needed now.

- [ ] M3.1 Define a small `Enemy` type using the properties already needed for drawing.
- [ ] M3.2 Create an enemy object and pass it to `drawEnemy`.
- [ ] M3.3 Replace positional drawing arguments with the object's properties, checking the picture afterward.
- [ ] M3.4 Repeat for a small `Tower` type and its object.
- [ ] M3.5 Change an object property and render again.
- [ ] M3.6 Explain which lines hold data and which lines turn that data into pixels.

**Understand:** type versus value, object literals, property access, and object references. A `const` object binding does not automatically make every property immutable; discuss that distinction using this object.

**Experiment:** intentionally misspell a property or provide text where a number belongs and inspect the editor's feedback, then fix it.

**Check:** drawing reads the object; the same position is not separately hardcoded inside the drawing function.

**Later expansion:** speed, health, IDs, and cooldowns arrive when behaviour needs them.

## M4 — Make one enemy move

**Visible goal:** an enemy moves along the lane without leaving a trail.

**Files:** `src/main.ts`. Introduce a frame callback and `updateEnemy(enemy, seconds)`.

- [ ] M4.1 Clear and redraw the static scene once from `render`.
- [ ] M4.2 Schedule repeated rendering with `requestAnimationFrame`, initially without movement.
- [ ] M4.3 Use consecutive callback timestamps to calculate elapsed seconds; handle the first frame explicitly.
- [ ] M4.4 Add enemy speed in pixels per second.
- [ ] M4.5 Write `updateEnemy` to return an updated enemy using speed multiplied by elapsed seconds.
- [ ] M4.6 Assign the returned enemy before rendering the next picture.
- [ ] M4.7 Try zero, double, and negative speed, then restore the intended direction.

**Understand:** callbacks, passing a function versus calling it, elapsed time, return values, and state reassignment. If object-spread syntax is used, explain its shallow-copy behaviour with the actual fields.

**Experiment:** predict the distance travelled in two seconds at a chosen speed, then roughly compare it with the game.

**Check:** the enemy moves and redraws cleanly. A drawing call itself does not advance the enemy.

**Later expansion:** this frame-based driver is a learning increment. M6 feeds the same movement rule a fixed tick duration and handles long browser interruptions.

## M5 — End an enemy's journey

**Visible goal:** the enemy disappears at the exit.

**Files:** `src/main.ts`. Introduce `hasReachedExit` and an explicit representation of an absent enemy.

- [ ] M5.1 Define the entrance and exit positions visibly on the lane.
- [ ] M5.2 Decide whether reaching the exit is measured from the enemy's centre or edge; record the choice.
- [ ] M5.3 Add `hasReachedExit` as a function returning a boolean.
- [ ] M5.4 Represent the enemy as absent after it exits and guard update/draw accordingly.
- [ ] M5.5 Check positions just before, exactly at, and just after the chosen exit boundary.

**Understand:** boolean expressions, conditions, optional/nullable state, and lifecycle transitions.

**Experiment:** move the exit closer and explain why the enemy disappears earlier without changing its speed.

**Check:** an enemy is not updated or drawn after removal. Reloading restores the initial scene at this stage.

**Later expansion:** M7 replaces this single-enemy representation with a collection. Lives are introduced at M12.

## M6 — Separate the parts we can now explain

**Visible goal:** the same moving-enemy scene works through a small simulation with fixed ticks.

This milestone contains several separate exchanges. First move working code, then adjust timing, then configure type checking. Verify between each part.

**New files, created only when reached:** `src/render.ts`, `src/sim/world.ts`, `src/sim/step.ts`, and project-root `tsconfig.json`.

- [ ] M6.1 Define a minimal `World` from the data that currently exists; add `createWorld` for a fresh starting state.
- [ ] M6.2 Put world data/types in `sim/world.ts` and reconnect them with explicit imports and exports.
- [ ] M6.3 Move drawing to `render.ts`; make `render(ctx, world)` read the world without changing game state.
- [ ] M6.4 Move movement and exit handling into `sim/step.ts`; have `step(world)` return the next world.
- [ ] M6.5 Keep browser lookups and scheduling in `main.ts`; confirm simulation modules have no canvas/DOM access or wall-clock reads.
- [ ] M6.6 Walk through a concrete fixed-tick example on paper, including a frame that needs zero ticks and one that needs two.
- [ ] M6.7 Add a fixed simulation duration and an elapsed-time accumulator in the browser loop. Track the simulation tick in the world.
- [ ] M6.8 Feed that fixed duration to movement; render after the required simulation steps.
- [ ] M6.9 Choose a bounded catch-up policy for long pauses. Check that returning from a background tab does not create a huge jump or long freeze.
- [ ] M6.10 Add a small Vite-compatible TypeScript configuration with `strict` and `noUncheckedIndexedAccess`, and a `typecheck` script using `tsc --noEmit`.
- [ ] M6.11 Run the type check and explain/fix errors one at a time.
- [ ] M6.12 Trace the complete update/render flow and confirm the scene still behaves as before.

**Understand:** module boundaries, imports/exports, one owner of current world state, and display frames versus simulation ticks. Fixed ticks alone are not a promise of cross-browser lockstep determinism.

**Experiment:** call the simulation for a known number of ticks and predict the enemy's new position. Explain why the renderer can draw the same world twice without moving anything.

**State convention:** prefer functions that return updated simulation data without mutating their input. Make the small copies explicit; do not introduce a generic state framework or deep-clone the entire world as a shortcut.

**Check:** type checking succeeds, drawing reads state, and simulation rules can be called without a canvas.

## M7 — Move several enemies independently

**Visible goal:** a small group crosses the lane; each enemy can disappear independently.

**Files:** existing world, step, and render modules. No new module required.

- [ ] M7.1 Replace the single-enemy field with an array in `World`.
- [ ] M7.2 Create a few initial enemies with different positions or speeds.
- [ ] M7.3 Update and render every enemy using a clear collection operation.
- [ ] M7.4 Remove enemies that reach the exit without skipping their neighbours.
- [ ] M7.5 Give enemies stable numeric IDs; allocate them from a counter in world state.
- [ ] M7.6 Verify that two enemies exiting in the same tick are both handled.

**Understand:** arrays, iteration, transformation versus filtering, and IDs versus array positions. Introduce `map`/`filter` through these concrete tasks rather than several competing styles at once.

**Experiment:** remove the middle enemy and inspect which IDs remain. Change one speed and observe independent movement.

**Check:** removing one enemy never causes another enemy to inherit its identity or miss its update.

## M8 — Spawn a finite wave over time

**Visible goal:** enemies enter the lane at intervals and the spawning eventually stops.

**New file:** `src/sim/waves.ts`, when extracting wave behaviour is useful. Introduce `spawnEnemy` and `updateWave`.

- [ ] M8.1 Describe one wave with enemy count, speed, and a spawn interval.
- [ ] M8.2 Add wave progress to the world: enemies still to spawn and ticks until the next spawn.
- [ ] M8.3 Use `spawnEnemy` to create a fresh enemy with a unique ID at the entrance.
- [ ] M8.4 Let `updateWave` advance the countdown and spawn at the chosen interval.
- [ ] M8.5 Stop spawning when the configured count is exhausted.
- [ ] M8.6 Record whether the first spawn is immediate or delayed; make the countdown follow that rule.
- [ ] M8.7 Verify the configured count actually spawns. Reject or explicitly handle a zero/negative interval.

**Understand:** configuration versus runtime progress, countdowns, function composition, and passing updated state from one rule to the next.

**Experiment:** compare a slow dense pack with a fast sparse stream. Change one value at a time.

**Check:** the wave has a finite size and consistent spacing. "No enemies currently on screen" does not yet mean the wave is finished if some are still scheduled.

**Design checkpoint:** choose an initial pace and enemy count. Record the values and the reason they felt useful.

## M9 — Find a target in range

**Visible goal:** a range circle surrounds the tower and the selected enemy is highlighted.

**New file:** `src/sim/combat.ts`. Introduce `isInRange` and `findTarget` as needed.

- [ ] M9.1 Add a range value and draw the matching circle.
- [ ] M9.2 Define a consistent distance rule, initially centre-to-centre with the boundary included.
- [ ] M9.3 Write `isInRange` and check inside, exactly on, and outside the boundary.
- [ ] M9.4 Have `findTarget` choose the in-range enemy furthest along the lane.
- [ ] M9.5 Return an enemy ID or an explicit no-target result; handle the empty case.
- [ ] M9.6 Make ties predictable, for example by choosing the lower ID.
- [ ] M9.7 Use the selection to highlight the target while keeping drawing free of state changes.

**Understand:** distance, comparisons, searching a collection, optional results, and referencing entities by ID. If squared distance is used, connect it to the visible circle before discussing optimisation.

**Experiment:** move the tower and alter its range. Predict the moment selection begins and ends.

**Check:** out-of-range enemies are never selected, and a missing target does not cause an error.

## M10 — Attack, take damage, and die

**Visible goal:** the tower damages and defeats enemies in the wave.

**Files:** world types, combat, step, and render. Introduce `updateTower`; extract `applyDamage` only if it makes the behaviour easier to follow.

- [ ] M10.1 Add enemy maximum/current health, tower damage, and an attack interval.
- [ ] M10.2 Add the tower's remaining cooldown to runtime state.
- [ ] M10.3 Count the cooldown down in simulation ticks without letting it become an accidental endless negative counter.
- [ ] M10.4 When ready, find a valid target, apply damage, and reset the cooldown only when a shot actually occurs.
- [ ] M10.5 Remove defeated enemies and exclude them from later targeting and escape handling.
- [ ] M10.6 Choose and record the tick order: spawn, move, handle escapes, attack, remove defeated enemies. An enemy already at the exit escapes before this tick's attack.
- [ ] M10.7 Add a minimal visible attack indication, then develop it further in M11.
- [ ] M10.8 Verify an empty lane, a surviving target, a killed target, and a target that leaves range.

**Understand:** coordinating state, countdowns, composing rules, and how operation order affects outcomes. Instant damage is the initial model; there is no separate projectile simulation yet.

**Experiment:** compare frequent weak hits and slow heavy hits with roughly similar average damage. Notice wasted damage and how enemy spacing changes the result.

**Check:** attacks obey the cooldown, kills are visible, and dead enemies cannot later count as escapes.

**Design checkpoint:** choose a first weapon feel. Keep moving projectiles as an explicit later choice if desired.

**Milestone reached:** one tower fights a finite moving wave. Pause here to explore and explain the game before adding more systems.

## M11 — Make combat easy to read

**Visible goal:** health bars and brief attack effects explain what is happening.

**Files:** existing simulation result types, combat, main, and render. A separate effects module is optional only if the current code needs it.

- [ ] M11.1 Draw a health bar based on current health divided by maximum health.
- [ ] M11.2 Handle/clamp the visible fraction and keep defeated enemies out of the normal render list.
- [ ] M11.3 Expand `step` to return `{ world, events }` and reconnect its caller before adding multiple event kinds.
- [ ] M11.4 Emit one `towerFired` event containing the IDs and positions needed to show the shot.
- [ ] M11.5 Consume each tick's events once in the browser/presentation layer, including frames that run several ticks.
- [ ] M11.6 Keep short-lived visual effects in presentation state with an explicit expiry; drawing itself remains read-only.
- [ ] M11.7 Verify that a shot still appears if its target died and was removed in the same tick.

**Understand:** enduring state versus something that happened, snapshot data, and separating visual lifetime from game rules.

**Experiment:** lengthen the beam/flash duration without changing damage or firing rate.

**Check:** visuals communicate combat and do not modify its outcome. Multiple simulation ticks in one rendered frame do not silently lose their events.

## M12 — Finish and restart one automatic round

**Visible goal:** the round ends in victory or defeat and restarts with fresh state.

**Files:** world, step, main, render, and `index.html` if using normal HTML controls. Reuse `createWorld`.

- [ ] M12.1 Add starting/current lives and subtract one for each escaped enemy exactly once.
- [ ] M12.2 Show lives in a simple HUD.
- [ ] M12.3 Add explicit phases: `ready`, `running`, `won`, and `lost`.
- [ ] M12.4 Add a start action and advance combat only in the running phase.
- [ ] M12.5 Define wave completion as no pending spawns and no active enemies.
- [ ] M12.6 Lose at zero lives; otherwise win when this single wave is complete. Give defeat priority if the final escape also empties the lane.
- [ ] M12.7 Display the result and prevent finished-round rules from continuing to run.
- [ ] M12.8 Restart through `createWorld`, clearing presentation effects, pending actions, and browser timing carry-over too.
- [ ] M12.9 Win, lose, and restart several times without reloading the page.

**Understand:** state transitions, mutually exclusive phases, conditions involving several fields, and resetting all the owners of runtime state.

**Experiment:** make an easy winning configuration and a losing one. Predict what should happen when the final enemy escapes with one life left.

**Check:** a gap between spawns is not a victory. Each escape costs one life. Restart does not add a second animation loop or duplicate event listeners.

**Later expansion:** simple start/restart handlers may initially call their dedicated functions. M13 turns game input into commands at a consistent boundary.

## M13 — Let the player choose a tower position

**Visible goal:** clicking a valid build spot places the tower before starting the round.

**New file:** `src/input.ts`. Extend `step(world, commands)`; introduce a small command type and `applyCommand` when useful.

- [ ] M13.1 Confirm the proposed interaction: one tower, three build spots, placement before the round.
- [ ] M13.2 Draw the spots and represent their positions as data.
- [ ] M13.3 Convert pointer coordinates from the browser rectangle into canvas coordinates, accounting for display scaling.
- [ ] M13.4 Translate a click into a placement command identifying the selected spot.
- [ ] M13.5 Queue commands and consume each once at the simulation boundary, even while in a ready phase.
- [ ] M13.6 Validate placement in the simulation: a known spot, the allowed phase, and the one-tower limit.
- [ ] M13.7 Move start/restart input into the same clear command flow; ensure restart clears stale queued input.
- [ ] M13.8 Choose whether repeated pre-wave clicks move the tower or are ignored; record and implement the rule.
- [ ] M13.9 Test valid clicks, empty-space clicks, clicks after starting, and clicking at a different displayed canvas size.

**Understand:** event handlers, coordinate conversion, commands as data, input queues, and simulation validation. Input describes a request; game rules decide whether it is allowed.

**Experiment:** compare entrance, middle, and exit placement against the same wave.

**Check:** one click is applied once, invalid input leaves the world valid, and the selected placement has a visible effect.

**Design checkpoint:** decide whether to keep build spots or plan free placement. A scope change should update the remaining checklist, not silently add several new systems.

## M14 — Make a three-wave level from data

**Visible goal:** place a tower, start and survive three waves, and reach a final result.

**New files:** `src/sim/definitions.ts` for reusable enemy/tower definitions, then `src/sim/level.ts` for this level's layout and waves. Introduce them in separate edits.

- [ ] M14.1 Extract existing enemy and tower stats into definitions with stable definition IDs.
- [ ] M14.2 Keep runtime values such as current health and remaining cooldown in the world; resolve static stats through definitions.
- [ ] M14.3 Put lane endpoints, build spots, starting lives, and wave descriptions into level data.
- [ ] M14.4 Make `createWorld` use the selected level and definitions; preserve the current behaviour first.
- [ ] M14.5 Add three wave descriptions and track the current wave index.
- [ ] M14.6 Expand wave completion: enter a between-waves phase when more waves remain, or win after the final one.
- [ ] M14.7 Add a start-next-wave command that is valid only in the between-waves phase.
- [ ] M14.8 Show the current wave and useful remaining-enemy information.
- [ ] M14.9 Decide whether tower repositioning is allowed between waves; keep it disabled unless deliberately chosen.
- [ ] M14.10 Finish the complete level and restart into the original ready state.

**Understand:** reusable definitions versus level content versus runtime state, indexed data with possible missing entries, and expanding existing functions without rewriting the game.

**Experiment:** change only the final wave's count, health choice, speed, or interval and verify the earlier waves stay the same. Add a second definition only if the experiment needs one.

**Check:** victory waits for the final wave to resolve. Starting the next wave early is rejected. Lives persist across waves and reset across new games.

**Design checkpoint:** choose the intended difficulty and level length by playing. Three waves are a starting proposal, not a permanent rule.

## M15 — Tune, verify, and explain the first level

**Visible goal:** a small level that is understandable, replayable, and deliberately tuneable.

**Files:** the implemented game plus focused test files beside simulation code where useful. Configure the existing Vitest dependency rather than adding a new testing framework.

- [ ] M15.1 Play the same level from each allowed tower position and record leaks/wins before changing balance.
- [ ] M15.2 Choose one balance problem, predict a change, alter one relevant value, and compare the result.
- [ ] M15.3 Make the lane, range, targeting, health, remaining lives, and result understandable at a glance.
- [ ] M15.4 Replace the placeholder test script with a real Vitest command.
- [ ] M15.5 Test range boundaries and the chosen targeting tie-break rule.
- [ ] M15.6 Test cooldown spacing and that a missing target does not consume a shot.
- [ ] M15.7 Test an escape costing one life and multiple same-tick escapes being counted correctly.
- [ ] M15.8 Test that scheduled-but-unspawned enemies prevent wave completion, and final-wave completion controls victory.
- [ ] M15.9 Test fresh-world reset isolation and, for the value-returning simulation contract, that a representative step leaves its input world unchanged.
- [ ] M15.10 Run type checking and the focused tests; record the actual results.
- [ ] M15.11 Manually check placement, start, between-waves input, win, loss, restart, and return from a background tab.
- [ ] M15.12 Explain one complete interaction: click -> command -> rule -> new world -> rendered result.
- [ ] M15.13 Record the first-level result, a screenshot location, important decisions, and one chosen next feature.

**Understand:** testing rules independently of the browser, boundary cases, evidence-led tuning, and explaining a change's consequences.

**Check:** the first-level acceptance list below is satisfied. Tests should protect meaningful behaviour; avoid tests that merely restate drawing instructions.

## First-level acceptance checklist

- [ ] The game starts from the existing project and displays the level without runtime errors.
- [ ] The player can make the agreed tower-placement choice and start the first wave.
- [ ] Enemies spawn, move, take damage, die, or escape consistently.
- [ ] Targeting and attacks follow the chosen range, priority, and cooldown rules.
- [ ] Lives and wave progress are visible and correct.
- [ ] All planned waves can be played, with a meaningful win and loss result.
- [ ] Restart produces a fresh playable game without refreshing the browser.
- [ ] The main balance values are easy to locate and experiment with.
- [ ] Type checking and the focused simulation tests pass.
- [ ] The user can follow the main state/data flow and identify the function to change for a small feature.
- [ ] The progress page contains enough context for the next chat to continue.

## How functions and files grow

These are planned responsibilities, not instructions to create everything in advance.

| Piece | First responsibility | Later expansion |
| --- | --- | --- |
| `main.ts` | Connect to the canvas | Own browser scheduling, current world, and wiring |
| `render(ctx, world)` | Draw the scene | Read entities, health, UI state, and presentation effects |
| `updateEnemy` | Move along a horizontal lane | Remain the movement rule when the path grows later |
| `createWorld` | Construct the initial state | Reset the selected level from its definitions |
| `step(world)` | Advance movement and exits | Become `step(world, commands) -> { world, events }` |
| `updateWave` | Spawn one finite group | Use level data and manage wave progress |
| `findTarget` | Choose the furthest eligible enemy | Accept a targeting preference if later requested |
| `updateTower` | Apply a ready attack | Support additional tower behaviour if chosen later |
| `input.ts` | Interpret a placement click | Translate browser actions into game commands |
| Definitions / level data | Hold values we already use | Support new content without duplicating rule code |

Avoid a generic engine, entity-component framework, or class hierarchy unless a later concrete need and learning decision justify it. Local, understandable changes are enough for this level.

## Gradually increasing independence

- **Early drawing and movement:** assistant supplies small working examples; user places them, asks questions, and changes values.
- **Collections and waves:** user fills in familiar expressions or predicts a function's result before seeing the next piece.
- **Combat and state transitions:** user takes a small function with described inputs and output; assistant gives hints or an example when useful.
- **Placement and tuning:** user increasingly owns a feature change, with review and debugging help.

This is flexible. Seeing a new API can still call for a supplied example late in the project. Understanding a familiar concept can justify moving quickly early in it.

## Later feature parking lot

Choose one after the first level, then make its own incremental plan. None of these is a hidden requirement for M15.

- Gold from kills, tower purchase costs, income, upgrades, and selling.
- Additional tower/enemy types and targeting preferences.
- Moving projectiles, area damage, elemental effects, and richer visuals.
- Free placement, curved or segmented paths, then maze building and pathfinding.
- Pause and speed controls that build on the separated simulation clock.
- Save/load, seeded randomness, replay tests, and headless balance experiments.
- Packaging and publishing a build friends can open from a link.
- Multiplayer and the later narrative/tools projects from the broader plan.

## References for the relevant implementation steps

- [Vite: HTML entry point and project root](https://vite.dev/guide/#index-html-and-project-root)
- [Vite: TypeScript and separate type checking](https://vite.dev/guide/features.html#typescript)
- [MDN: requestAnimationFrame and elapsed-time animation](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame)

Use these when a question arises; reading all documentation before drawing the first shape is not a prerequisite.
