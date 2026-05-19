# cat-vector-editor

Interactive SVG of a cat whose pupils subtly track the cursor.

- [`cat.svg`](cat.svg) — the standalone interactive SVG. Open it directly in a browser. Contains an embedded `<script>` that converts cursor position to SVG coordinates and translates each pupil toward the cursor with a smooth falloff (more horizontal than vertical travel, since the pupils are vertical slits).
- [`index.html`](index.html) — a thin wrapper that embeds the SVG via `<object>` and forwards window-level `mousemove` events into the inner document, so the pupils track from anywhere on the page. Suitable for GitHub Pages.

## How it works

Each pupil `<rect>` is wrapped in a `<g id="leftPupil">` / `<g id="rightPupil">`. The script:

1. Listens for `mousemove` / `touchmove` on the window.
2. Maps client coordinates to SVG coordinates via `svg.getScreenCTM().inverse()`.
3. Computes the vector from each eye center to the cursor, scales it by `min(1, dist / SATURATE)`, and clamps the result to `MAX_X` / `MAX_Y` (11 / 4 SVG units).
4. Applies the result as a `translate(...)` on the wrapping `<g>`. A CSS `transition` smooths the motion.

Eye centers were read off the source paths: `(436.918, 446.501)` and `(699.275, 446.501)` in the 1600×1200 viewBox.

## Tuning

Edit the constants near the top of the `<script>` block in `cat.svg`:

| Constant   | Effect                                                                |
| ---------- | --------------------------------------------------------------------- |
| `MAX_X`    | Maximum horizontal pupil offset, in SVG units.                        |
| `MAX_Y`    | Maximum vertical pupil offset. Keep small — pupils are tall slits.    |
| `SATURATE` | Distance (in SVG units) at which the offset reaches its max.          |

## License

Original illustration: third-party Dribbble shot used as input. Interactivity © Rikki Janae.
