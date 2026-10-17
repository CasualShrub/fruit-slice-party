# Fruit Slice Party

A simple mobile game by Shrub (indie dev). Release targets: **iOS App Store, Google Play, Amazon Appstore**.

## Core gameplay
- Fruits spawn at the top of the screen and fall downward, each with a random angle and spin.
- A **dotted line** sits near the bottom of the screen.
- The player **taps anywhere** to slice the fruit along the dotted line at that moment.
- Goal: cut the fruit into **perfect halves**. After each slice, show the split as two percentages, e.g. `48% / 52%` (great) or `10% / 90%` (tapped too early, just as the fruit reached the line).
- The split is the ratio of the fruit's **area** on each side of the line at the moment of the tap. Fruits are rotated, so measure the actual shape, not its bounding box.
- Keep controls one-touch: no swiping, no menus during play.

### Order queue
The point is to make players keep track of what they need next, so the game isn't just slicing everything on repeat.
- The screen always shows **three fruit symbols**: the fruits the player is currently asked to slice (e.g. mango, banana, banana). They are generated randomly when the level loads.
- Slicing a fruit that matches **any** of the symbols clears one matching symbol, awards points (see Scoring), and **instantly** generates a new symbol to replace it, so there are always three.
- Slicing a fruit that **isn't** in the queue gives a **strike**. **Three strikes end the run.** Wrong slices don't cost points.
- A tap slices **every** fruit on the dotted line at that moment. When a wanted and an unwanted fruit cross together, the player can hold off and skip both rather than take a strike. Make sure fruits pass the line together often enough that this choice actually comes up.
- Fruits that aren't in the queue keep spawning as distractors. Make sure the wanted fruits still show up often enough that the queue can be cleared.

### Scoring
Points depend on how far the split is from 50/50. Deviation = `abs(larger_side% - 50)`. A 52/48 split has a deviation of 2.

| Deviation | Split range | Points |
|---|---|---|
| ≤ 0.5 | 50.5/49.5 or better | **150** (absolutely perfect) |
| ≤ 2 | 52/48 or better | **100** (perfect) |
| > 2 | worse than 52/48 | scales down linearly from 100 at a deviation of 2 to 0 at a deviation of 50 |
- Any wanted fruit that gets sliced clears its symbol, even a bad split.
- Keep the thresholds and the falloff as tunable values. The falloff curve is a placeholder until it's been playtested.

### Difficulty
- Spawn rate and fall speed both **increase over time** during a run.

## Art direction
- **Very, very simple.** Blob shapes and childlike MS Paint drawings: thick uneven outlines, flat colors, wobbly lines, no gradients or shading.
- Reference: [docs/art-reference/tomato.png](docs/art-reference/tomato.png) is a hand-drawn tomato (thick red outline, green stem on a black background). Match this look.
- Keep UI text and effects in the same handmade style. Don't polish it.

## Tech
- **Godot 4.7.2**, Mobile renderer. Use **GDScript** with static typing (`var speed: float`, typed function signatures).
- The Godot project lives in the `fruit-slice-party/` subfolder (`res://` = `fruit-slice-party/`). The repo root holds docs, the license (Apache 2.0) and this file.
- Stretch mode is `canvas_items` / `expand`. Design for **portrait** phones, and make the layout survive different aspect ratios and notches/safe areas.
- 2D only.

## Conventions
- Files and folders: `snake_case` (`fruit.gd`, `scenes/fruit.tscn`). Node names and `class_name`: `PascalCase`.
- Keep one scene per gameplay object (fruit, dotted line, HUD, etc.) and wire them up with signals instead of reaching across the tree.
- Put tunable values (fall speed, spawn rate, spin range, line position, scoring thresholds) in `@export` vars or a single config resource, not as magic numbers.
- Don't hand-edit UIDs in `.tscn`/`.tres` files, and don't change `project.godot` without saying so.

## Checking work
- Run `godot --headless --path fruit-slice-party --quit` to confirm the project loads and scripts parse (`godot` must be on PATH).

## Store notes
- Avoid anything that differs per store unless it's isolated behind one platform layer (ads, IAP, leaderboards). Amazon builds use the Android export.
- **Ads and leaderboards are planned.** Each store has a different service (Game Center on iOS, Play Games on Google Play, and Amazon has no current equivalent, so it may need a cross-platform backend), so put them behind one interface and keep gameplay code unaware of which store it's on.
- **In-app purchases:** to be decided later. Don't build them yet.

## Open decisions (ask before assuming)
- The fruit roster, and how many fruit types can show up at once.
