# CLAUDE.md · Cage Rush

Cage Rush is an arcade MMA fighting game for the browser. Fast, punchy, fun. It has quick play, tournaments, a daily fight, a practice range and a deep career mode.

## The codebase
- The whole game lives in one file: `index.html`. That one file holds the HTML, CSS and JavaScript. Keep it that way: no build step, no frameworks, no extra files the game needs to load.
- The only external script is three.js r128 from cdnjs, and it only loads lazily when the optional 3D beta is on. Fonts come from Google Fonts. Do not add other dependencies.
- Everything renders on a 960x540 canvas (`W`, `H`, floor line `G`) that scales to fit the screen. Menus and overlays are HTML on top of the canvas.
- Saves live in `localStorage` through the `save` object and `persist()`. Never break old saves. When you add a field, default it lazily (for example `c.wt==null`), not with a save wipe.

## Map of the file (search for these names)
- **Fighters:** `mkF` builds a fighter. Visual size comes from `look.scale`.
- **Combat:** `startMove`, `resolveHit`, `upd`, `step` (the main loop), `endRound` and `decision` (judges' scoring).
- **AI:** `aiThink` for standing, `aiGround` for the ground, clinch AI next to `clinchStep`.
- **Ground game:** `startGround`, `groundStep`, `placeTop`, plus clinch (`startClinch`) and kick catches (`kc`).
- **Moves:** the `MOVES` table, plus the `PUNCHES` and `KICKS` lists.
- **Career:** `currentOpp`, `makeOffers`, `takeOffer`, `careerPost`, `careerOppScale` (difficulty), `DIVS` (weight classes).
- **Weight:** `wtFx`, `WPLAN`, `sizeScale`, `oppSize`, `changeDiv`.
- **Camp:** `SESS`, `campFx`, `renderCamp2`, and the drills in `CDRILL`/`openDrill`.
- **Pre-fight:** `openFweek` (fight week, weigh-in), `showTape` (tale of the tape, mic, stare-down).
- **Between rounds:** the corner (`ADJ` adjustments, `OBJ` objectives).
- **Unlocking moves:** `MUNLOCK` unlocks advanced strikes by wins plus mastery. There is no dojo gating anymore.
- **UI skin:** the CSS between `/*SKIN*/` and `/*ENDSKIN*/`. Gold `--hot` primary buttons, one card style.
- **3D beta:** the block starting `/* ===== 3D RENDERER (three.js) =====`. Off by default (`save.r3d`).

## Design rules (do not break these)
- Arcade first: fast, readable, fun. Keep the floating damage numbers and the "N HIT" combo popups.
- Must work on PC (keyboard) and mobile (touch, landscape phones down to about 667x375). Check both after UI changes.
- Our own art, drawn in code. No emojis in the UI, no copied characters, logos or assets. References are inspiration only.
- Game text and UI copy: conversational, short, and no em dashes.
- 2D at its core. 3D is an optional beta setting and stays off by default.
- Career should be challenging. No random arena gimmicks in career (big head, low gravity and so on are quick-play only). Wins should come from skill and preparation, not luck.
- Striking must not beat everything. Combo spam has diminishing returns, wrestling scores on the cards, and the AI reads repeated patterns. Keep that balance.

## How to work in this repo
- Make small, focused changes. One feature or fix per pull request, because two big edits to one file will conflict.
- Before you finish, open `index.html` in a browser (or headless Chromium) and check there are no console errors. Then play a quick fight and, if you touched career, walk through one career fight.
- Do not reformat or minify the file. Many lines are long on purpose, and reformatting creates huge diffs that break merges.
- Explain in plain words what you changed and how to test it.
