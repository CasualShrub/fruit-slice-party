# Fruit Slice Party

A simple mobile game by Shrub (indie dev). Release targets: **iOS App Store, Google Play, Amazon Appstore**.

## Core gameplay
- Fruits spawn at the top of the screen and fall downward, each with a random angle and spin.
- A **dotted line** sits near the bottom of the screen.
- The player **taps anywhere** to slice the fruit along the dotted line at that moment.
- Goal: cut the fruit into **perfect halves**. After each slice, show the split as two percentages, e.g. `48% / 52%` (great) or `10% / 90%` (tapped too early, just as the fruit reached the line).
- The split is the ratio of the fruit's **area** on each side of the line at the moment of the tap. Fruits are rotated, so measure the actual shape, not its bounding box.
- Keep controls one-touch: no swiping, no menus during play.

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

## Open decisions (ask before assuming)
- Scoring, lives, and game-over rules.
- Whether fruits speed up or spawn faster over time.
- Monetization (ads/IAP) and any leaderboards.
