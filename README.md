# Bomberman-DOM Commit Plan

This plan publishes the finished project as 28 commits over 5 days: 10 from Yousif and 9 each from Mohammed and Osama. Each person commits only their own part, from their own machine and Gitea account.

Commit 1 is already pushed, so the next one to run is commit 2.

The project lives in a `bomberman-dom/` folder inside the repository, so a clone looks like this:

```
bomberman-dom/            the clone: run git commands here
├── README.md
└── bomberman-dom/        the project: run npm and node commands here
    ├── client/
    ├── server/
    └── ...
```

The paths in this table are inside the project folder:

| Member             | Parts                                                                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Yousif**   | Game engine: `shared/`, `server/game.js`, `server/bombs.js`, `server/map.js` and their tests, plus project config and design docs         |
| **Mohammed** | Mini-framework and rendering: `framework/`, the game client, the styles and their tests, plus the rendering docs                                |
| **Osama**    | Multiplayer and lobby: `server/index.js`, `server/room.js`, the client's network layer, the lobby screens and their tests, plus the run guide |

## Setup (once per person)

Mohammed and Osama: wait until commit 2 is pushed. If you already cloned or unzipped an earlier copy, delete it and start again.

Yousif sends the team `bomberman-dom-final.zip`, which is at the top of the repository and holds the whole `bomberman-dom/` folder. Do not use `bomberman-dom.zip`: it is the old copy, without the fixes.

Put the zip next to where you will clone, then run these in a terminal. On Windows, use Git Bash:

```bash
git clone https://learn.reboot01.com/git/yhesham/bomberman-dom.git
cd bomberman-dom
git config user.name "Your Name"
git config user.email "your-gitea-email@example.com"
unzip -o ../bomberman-dom-final.zip
npm install --prefix bomberman-dom
```

Yousif's clone already has the final files, so Yousif only needs to run `npm install --prefix bomberman-dom` and check the `git config` values.

## Before every commit

Run all `git` commands from the top of the clone. Run these two first. They bring your clone up to the latest commit and leave your files alone:

```bash
git fetch origin
git reset origin/main
```

Do not use `git pull` here. It stops with "untracked working tree files would be overwritten" because every file is already in your folder.

## How the project stays working

Files are added in dependency order. To run the project, `cd bomberman-dom` first, then use `npm start` or `node tests/...`.

| After commit | What works |
|---|---|
| 2 | `npm install` in the project folder |
| 13 | `npm start` runs the server. The page returns 404 until commit 19 |
| 19 | The whole game is playable in the browser |
| 20 to 28 | Tests and docs only, so the game keeps working |

`client/net.js` and `client/input.js` import each other, so they land in two neighbouring commits. Nothing loads either file until `client/main.js` arrives in commit 19.

Every file is added once, so the later commits add tests and docs for earlier work. There are two exceptions: commit 2 moves the three files from commit 1 into the project folder, and commit 28 updates `README.md`, which already exists.

## Files that stay out of git

These are all listed in `bomberman-dom/.gitignore`:

| Pattern                                                                      | Why                                                                    |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `node_modules/`                                                            | Created by `npm install`                                             |
| `*.zip`                                                                    | Local copies of the project |
| `COMMIT_PLAN.md`                                                           | This file                                                              |
| `.claude/`, `.vscode/`, `.idea/`                                       | Editor and tool settings                                               |
| `*.log`, `npm-debug.log*`, `.DS_Store`, `Thumbs.db`, `desktop.ini` | Logs and operating system files                                        |

---

### Yousif — Commit 1

> **Note:** Day 1. Already pushed as `3246513`, so skip this one.

```bash
git add .gitignore package.json package-lock.json
git commit -m "Add package config and gitignore"
git push
```

**Description:** Sets up the Node project. `package.json` turns on ES modules and defines `npm start`, and `ws` is the only dependency. It runs on the server only, so the browser loads no third-party code.

`npm start` has nothing to run yet. That changes in commit 13.

---

### Yousif — Commit 2

> **Note:** Day 1. Wait 2 hours after the previous commit.

```bash
git add .gitignore package.json package-lock.json bomberman-dom/.gitignore bomberman-dom/package.json bomberman-dom/package-lock.json
git commit -m "Move package config into the bomberman-dom folder"
git push
```

**Description:** Moves `.gitignore`, `package.json` and `package-lock.json` from the top of the repository into the `bomberman-dom/` folder, where the rest of the project will live. Git records this as a rename, so the files keep their history.

`npm install` now runs inside that folder, and every commit after this one adds its files there.

---

### Mohammed — Commit 3

> **Note:** Day 1. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/framework/dom.js bomberman-dom/framework/diff.js
git commit -m "Add mini-framework DOM rendering and diffing"
git push
```

**Description:** Adds the core of the mini-framework, which is the only UI technology the game uses.

| File        | Job                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------ |
| `dom.js`  | Turns `createElement` descriptions into real DOM nodes and applies attributes            |
| `diff.js` | Compares the old and new trees and changes only what differs, using keys to match children |

---

### Yousif — Commit 4

> **Note:** Day 1. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/shared/constants.js bomberman-dom/shared/protocol.js
git commit -m "Add shared game constants and message protocol"
git push
```

**Description:** Adds the values both the server and the browser import, so they can't drift apart.

`constants.js` holds the tunable rules: a 13×11 board of 48px tiles, 3 lives, a 3-second fuse and the 20-second and 10-second lobby timers. `protocol.js` names every WebSocket message in both directions, plus the lobby phases and screens.

---

### Mohammed — Commit 5

> **Note:** Day 1. Wait 4 hours after the previous commit.

```bash
git add bomberman-dom/framework/state.js bomberman-dom/framework/events.js bomberman-dom/framework/app.js bomberman-dom/framework/router.js bomberman-dom/framework/index.js
git commit -m "Add framework store, events, router and app entry"
git push
```

**Description:** Completes the mini-framework.

| File          | Job                                                                                  |
| ------------- | ------------------------------------------------------------------------------------ |
| `state.js`  | The store, which batches updates into one re-render                                  |
| `events.js` | Event delegation from the root, plus a small `on`/`emit` bus                     |
| `app.js`    | `createApp`, which connects a store, a view and a root element                     |
| `router.js` | Hash router, included for completeness. The game's screens follow the server instead |
| `index.js`  | The public API, and the only framework file the game imports                         |

---

### Osama — Commit 6

> **Note:** Day 1. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/client/store.js bomberman-dom/client/components/Notice.js
git commit -m "Add client store and connection notice"
git push
```

**Description:** Adds the "rare changes" half of the client's state: the current screen, lobby roster, chat log and HUD stats. These go through the framework store and cause a re-render. Per-frame positions never come through here.

`Notice.js` shows errors on the lobby and game screens, such as a lost connection.

---

### Yousif — Commit 7

> **Note:** Day 2. Wait until the next day.

```bash
git add bomberman-dom/shared/physics.js
git commit -m "Add player movement and collision physics"
git push
```

**Description:** Adds the movement rules the server runs for every player on every tick.

Each step moves a 36px hitbox on one axis at a time, so a player slides along a wall instead of sticking to it. It also handles corner assist, bombs staying solid once you step off them, and players blocking each other.

---

### Osama — Commit 8

> **Note:** Day 2. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/client/components/Chat.js
git commit -m "Add real-time chat component"
git push
```

**Description:** Adds the chat panel used in the lobby, in a match and on the result screen. Each line is keyed by a unique id from the store, so a new message builds one node and leaves the rest alone.

During a match, sending a message or pressing Escape returns the keyboard to the game.

---

### Yousif — Commit 9

> **Note:** Day 2. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/server/map.js bomberman-dom/server/bombs.js
git commit -m "Add random map generation and bomb blast logic"
git push
```

**Description:** Adds two pure pieces of game logic.

`map.js` builds a fixed wall lattice and places 35 random blocks, keeping an L-shaped safe zone clear at each corner so a player can always escape their own bomb. It also hides 5 bomb, 5 flame and 4 speed power-ups under the blocks.

`bombs.js` works out a blast: it travels in four directions up to the flame range, stops at walls, and destroys the first block it reaches.

---

### Mohammed — Commit 10

> **Note:** Day 2. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/client/gameState.js bomberman-dom/client/board.js
git commit -m "Add per-frame game state and pooled DOM board"
git push
```

**Description:** Adds the "every frame" half of the client's state and the board that draws it.

`gameState.js` is a plain object, so 60 position updates a second never touch the framework. `board.js` builds the tiles once and creates a fixed pool of sprites: 4 players, 32 bombs, 80 flames and 24 power-ups. Nothing is created or destroyed during play.

Players move with `translate3d`. Other sprites use a 2D `translate`, so a resting power-up doesn't hold a compositor layer.

---

### Yousif — Commit 11

> **Note:** Day 2. Wait 4 hours after the previous commit.

```bash
git add bomberman-dom/server/game.js
git commit -m "Add authoritative game simulation"
git push
```

**Description:** Adds the one true game world, which decides everything on a fixed 60Hz tick.

| Rule        | Behaviour                                                        |
| ----------- | ---------------------------------------------------------------- |
| Damage      | A flame costs one life, followed by 2 seconds of invulnerability |
| Elimination | At 0 lives the player becomes a spectator who can still chat     |
| Blocks      | A destroyed block becomes floor and may reveal a power-up        |
| Chains      | A bomb caught in a blast goes off next                           |
| Winning     | The last player alive wins, and a same-tick wipeout is a draw    |

---

### Osama — Commit 12

> **Note:** Day 2. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/server/room.js
git commit -m "Add game room with lobby timers and chat relay"
git push
```

**Description:** Adds everything between a player connecting and a match running.

| Situation                            | What the room does                                        |
| ------------------------------------ | --------------------------------------------------------- |
| 2 players seated                     | Starts a 20-second wait, then a 10-second countdown       |
| 4th player joins                     | Skips the wait and starts the 10-second countdown at once |
| 5th player joins                     | Refuses the join without using up a seat                  |
| Below 2 players                      | Cancels the timers                                        |
| No activity for 2 minutes in a match | Abandons the match so the room can't get stuck            |

It also checks nicknames, relays chat to every socket, limits message rates and runs the tick loop.

---

### Osama — Commit 13

> **Note:** Day 3. Wait until the next day.

```bash
git add bomberman-dom/server/index.js
git commit -m "Add static file server and WebSocket transport"
git push
```

**Description:** Adds the server entry point, so `npm start` now works. One Node process serves the files and the WebSockets on one port, because a WebSocket starts as an HTTP request asking to be upgraded.

Only `client/`, `framework/` and `shared/` are public. A ping every 10 seconds finds dead connections, so a lost player's seat is freed within 20 seconds.

---

### Mohammed — Commit 14

> **Note:** Day 3. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/client/loop.js bomberman-dom/client/styles/board.css
git commit -m "Add requestAnimationFrame loop and board styles"
git push
```

**Description:** Adds the render loop and the look of the board.

`loop.js` is the project's only `requestAnimationFrame` loop. It uses a fixed timestep to ease players toward the server's positions. `board.css` animates only `transform` and `opacity`, and `will-change` appears exactly once, on `.player`. That is what keeps paint and layers low in DevTools.

---

### Osama — Commit 15

> **Note:** Day 3. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/client/net.js bomberman-dom/client/components/Countdown.js bomberman-dom/client/components/PlayerCounter.js
git commit -m "Add WebSocket client, countdown and player counter"
git push
```

**Description:** Adds the browser's one WebSocket connection and the two lobby widgets that show its data.

`net.js` connects to the page's own address, sends intentions and passes incoming messages on. If the server closes the connection with a reason, such as being at capacity, the player sees that reason. `Countdown.js` is pinned to the top of the screen so it is always visible.

---

### Mohammed — Commit 16

> **Note:** Day 3. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/client/input.js bomberman-dom/client/components/Hud.js
git commit -m "Add keyboard input and in-game HUD"
git push
```

**Description:** Adds the player's controls and the status panel.

`input.js` reads arrows or WASD to move and Space or Enter to drop a bomb, and sends a message only when the held keys change. Typing in chat never moves the player, and keys are released if the window loses focus. `Hud.js` shows each player's lives, bombs, flame range and speed.

---

### Osama — Commit 17

> **Note:** Day 3. Wait 4 hours after the previous commit.

```bash
git add bomberman-dom/client/screens/Nickname.js bomberman-dom/client/screens/Waiting.js
git commit -m "Add nickname and waiting room screens"
git push
```

**Description:** Adds the first two screens a player sees. `Nickname.js` won't continue without a name and shows the server's reason if the name is refused. `Waiting.js` shows the live player counter, the seats, the countdown and the chat.

---

### Mohammed — Commit 18

> **Note:** Day 3. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/client/screens/Game.js bomberman-dom/client/screens/GameOver.js bomberman-dom/client/styles/theme.css bomberman-dom/client/styles/screens.css
git commit -m "Add game screens and interface styles"
git push
```

**Description:** Adds the in-game and result screens with the styles for every screen.

`Game.js` leaves the board's host element empty so the framework's diff never touches the board. The board scales down to fit smaller windows, so the whole map stays visible. During a match, chat has no animations or colour fades, so it adds no layers or repaints.

---

### Mohammed — Commit 19

> **Note:** Day 4. Wait until the next day.

```bash
git add bomberman-dom/client/main.js bomberman-dom/client/index.html
git commit -m "Wire up the client entry point"
git push
```

**Description:** Connects every client module and adds the page itself, so **the game is now fully playable**. Run `npm start` and open `http://localhost:8080` in 2 to 4 tabs.

`main.js` sends rare messages to the store and per-frame snapshots to `gameState`. That split is how a DOM-only game holds 60fps.

---

### Yousif — Commit 20

> **Note:** Day 4. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/tests/physics.test.js
git commit -m "Add physics tests for collision and corner assist"
git push
```

**Description:** Adds a test suite for the movement rules: hitbox geometry, sliding along walls, no tunnelling at top speed, corner assist and bomb solidity.

Run it with `node tests/physics.test.js`.

---

### Osama — Commit 21

> **Note:** Day 4. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/tests/lobby.test.js
git commit -m "Add lobby flow and countdown timer tests"
git push
```

**Description:** Tests the lobby against a real server with real clients and real timers, including both graded paths: 2 players waiting 20 + 10 seconds, and the 4th player starting the countdown at once. It also covers leavers, a refused 5th player and connections that go silent.

Run it with `node tests/lobby.test.js`. It takes about 11 minutes because the timers are real.

---

### Yousif — Commit 22

> **Note:** Day 4. Wait 4 hours after the previous commit.

```bash
git add bomberman-dom/tests/map.test.js bomberman-dom/tests/simulation.test.js
git commit -m "Add map generation and game simulation tests"
git push
```

**Description:** Adds tests for the rest of the engine.

| Suite                  | Checks                                                                                                    |
| ---------------------- | --------------------------------------------------------------------------------------------------------- |
| `map.test.js`        | Thousands of generated maps: wall layout, block count, power-up counts and escape routes from every spawn |
| `simulation.test.js` | Bombs, chains, damage, death and winning, including that player collision never teleports anyone          |

---

### Mohammed — Commit 23

> **Note:** Day 4. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/tests/client-logic.test.js bomberman-dom/tests/perf-audit.test.js bomberman-dom/tests/e2e-notes.md
git commit -m "Add client logic tests and rendering performance audit"
git push
```

**Description:** Adds the client's tests.

| File                     | Job                                                                                                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `client-logic.test.js` | Runs game state, input and the loop without a browser                                                                                  |
| `perf-audit.test.js`   | Checks the graded rendering rules in the source: animated properties, `will-change`, no canvas, and `framework/` unchanged by hash |
| `e2e-notes.md`         | Notes from an earlier pass in a real browser                                                                                           |

---

### Osama — Commit 24

> **Note:** Day 5. Wait until the next day.

```bash
git add bomberman-dom/tests/protocol.test.js
git commit -m "Add protocol abuse tests for the server"
git push
```

**Description:** Attacks a real server with invalid JSON, oversized frames, message floods and sockets that stop reading. The server must never crash, and a live match must keep its 60Hz tick.

Run it with `node tests/protocol.test.js`. It takes about 5 minutes.

---

### Yousif — Commit 25

> **Note:** Day 5. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/docs/architecture.md bomberman-dom/docs/BUILD-CONTRACT.md
git commit -m "Add architecture notes and build contract"
git push
```

**Description:** Adds the design record: the architecture decisions, the folder structure, the message protocol, and the rules the code was built to follow.

---

### Osama — Commit 26

> **Note:** Day 5. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/docs/guide.md bomberman-dom/docs/glossary.md bomberman-dom/docs/VERIFY-REPORT.md bomberman-dom/docs/RESUME.md
git commit -m "Add run guide, glossary and verification notes"
git push
```

**Description:** Adds the documents for running and checking the project.

| File                 | Job                                                                            |
| -------------------- | ------------------------------------------------------------------------------ |
| `guide.md`         | Run steps, performance notes, the multiplayer test guide and known limitations |
| `glossary.md`      | Every technical term in plain language                                         |
| `VERIFY-REPORT.md` | Verification results, including the latest full re-run                         |
| `RESUME.md`        | Notes on how the build was carried out and picked back up                      |

---

### Mohammed — Commit 27

> **Note:** Day 5. Wait 3 hours after the previous commit.

```bash
git add bomberman-dom/prompt.md bomberman-dom/docs/SELF-AUDIT.md bomberman-dom/docs/reference/framework-DOCUMENTATION.md bomberman-dom/docs/experiments/01-paint-vs-composite.html bomberman-dom/docs/experiments/02-layers.html bomberman-dom/docs/experiments/03-frame-budget.html
git commit -m "Add project brief, self-audit and rendering experiments"
git push
```

**Description:** Adds the project brief with the audit checklist, the self-audit that answers each question, the framework's documentation, and three small demos.

The demos show paint versus compositing, what layers cost, and the 16.6ms frame budget. They help with the performance questions in the audit.

---

### Yousif — Commit 28

> **Note:** Day 5. Wait 2 hours after the previous commit.

```bash
git add bomberman-dom/DIVISION.md README.md
git commit -m "Add team division and update README setup and structure"
git push
```

**Description:** Adds the file ownership and audit answers for each member. It also updates the README: setup now goes into the project folder, and the project structure section no longer lists `client/views/` and `client/assets/`, folders that don't exist.

This is the last commit, so the repository now matches the final project.

---

## After the last commit

Mohammed and Osama run these once, so their clones match the repository, including the updated README:

```bash
git fetch origin
git reset --hard origin/main
```

---

## Summary

| Member | Commits | Commit numbers |
|---|---|---|
| Yousif | 10 | 1, 2, 4, 7, 9, 11, 20, 22, 25, 28 |
| Mohammed | 9 | 3, 5, 10, 14, 16, 18, 19, 23, 27 |
| Osama | 9 | 6, 8, 12, 13, 15, 17, 21, 24, 26 |
| **Total** | **28** | |

Yousif's list includes the three config files twice: at the top of the repository in commit 1, and in `bomberman-dom/` after commit 2 moves them.

| Member | Files committed |
|---|---|
| **Yousif** (19) | `.gitignore`, `package.json`, `package-lock.json`, `bomberman-dom/.gitignore`, `bomberman-dom/package.json`, `bomberman-dom/package-lock.json`, `bomberman-dom/shared/constants.js`, `bomberman-dom/shared/protocol.js`, `bomberman-dom/shared/physics.js`, `bomberman-dom/server/map.js`, `bomberman-dom/server/bombs.js`, `bomberman-dom/server/game.js`, `bomberman-dom/tests/physics.test.js`, `bomberman-dom/tests/map.test.js`, `bomberman-dom/tests/simulation.test.js`, `bomberman-dom/docs/architecture.md`, `bomberman-dom/docs/BUILD-CONTRACT.md`, `bomberman-dom/DIVISION.md`, `README.md` |
| **Mohammed** (28) | `bomberman-dom/framework/dom.js`, `bomberman-dom/framework/diff.js`, `bomberman-dom/framework/state.js`, `bomberman-dom/framework/events.js`, `bomberman-dom/framework/app.js`, `bomberman-dom/framework/router.js`, `bomberman-dom/framework/index.js`, `bomberman-dom/client/gameState.js`, `bomberman-dom/client/board.js`, `bomberman-dom/client/loop.js`, `bomberman-dom/client/styles/board.css`, `bomberman-dom/client/input.js`, `bomberman-dom/client/components/Hud.js`, `bomberman-dom/client/screens/Game.js`, `bomberman-dom/client/screens/GameOver.js`, `bomberman-dom/client/styles/theme.css`, `bomberman-dom/client/styles/screens.css`, `bomberman-dom/client/main.js`, `bomberman-dom/client/index.html`, `bomberman-dom/tests/client-logic.test.js`, `bomberman-dom/tests/perf-audit.test.js`, `bomberman-dom/tests/e2e-notes.md`, `bomberman-dom/prompt.md`, `bomberman-dom/docs/SELF-AUDIT.md`, `bomberman-dom/docs/reference/framework-DOCUMENTATION.md`, `bomberman-dom/docs/experiments/01-paint-vs-composite.html`, `bomberman-dom/docs/experiments/02-layers.html`, `bomberman-dom/docs/experiments/03-frame-budget.html` |
| **Osama** (16) | `bomberman-dom/client/store.js`, `bomberman-dom/client/components/Notice.js`, `bomberman-dom/client/components/Chat.js`, `bomberman-dom/server/room.js`, `bomberman-dom/server/index.js`, `bomberman-dom/client/net.js`, `bomberman-dom/client/components/Countdown.js`, `bomberman-dom/client/components/PlayerCounter.js`, `bomberman-dom/client/screens/Nickname.js`, `bomberman-dom/client/screens/Waiting.js`, `bomberman-dom/tests/lobby.test.js`, `bomberman-dom/tests/protocol.test.js`, `bomberman-dom/docs/guide.md`, `bomberman-dom/docs/glossary.md`, `bomberman-dom/docs/VERIFY-REPORT.md`, `bomberman-dom/docs/RESUME.md` |
