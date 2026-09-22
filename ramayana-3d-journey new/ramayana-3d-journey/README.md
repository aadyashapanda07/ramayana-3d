# Ramayana — a cinematic 3D journey

A single-page, scroll-driven 3D experience built with Three.js. No build step,
no dependencies to install — it's one self-contained HTML file.

## Run it

Just open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari).

Double-clicking the file works in most browsers. If your browser blocks
ES module imports from a `file://` path, serve the folder locally instead:

```
# from inside this folder
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

or, with Node installed:

```
npx serve .
```

## What's inside

- `index.html` — everything: markup, CSS and the Three.js scene/animation code.
- Three.js is loaded at runtime from a CDN (`cdn.jsdelivr.net`), and three
  Google Fonts (Cinzel, EB Garamond, Tiro Devanagari Hindi) are loaded from
  `fonts.googleapis.com`. An internet connection is needed the first time
  it loads; there are no other external assets — all textures (glow, cloud,
  dust, ground grain) are generated on the fly with `<canvas>`.

## Characters

Low-poly figures built entirely from primitives (no external models), rigged
so `animate()` can walk their legs, sway their arms, and breathe them:

- **Rama, Lakshmana and Sita** — a trio, placed twice: leaving Ayodhya for
  exile (walking), and reunited at the Return (standing).
- **Hanuman** — mid-leap above the ocean between Hanuman's chapter and Ram
  Setu, and again standing watch near Lanka.
- **Sugriva and Bali** — near the mountains, referencing Kishkindha.
- **The vanara sena** — an instanced crowd of ~50 monkey-warriors scattered
  along the mountains and the bridge, each with slight color and size
  variation.

Find them in the script under the `CHARACTERS` comment block: `buildFigure()`
is the rig builder, `heroTrio()` / the individual `buildFigure(...)` calls
place them in the world, and `animateFigure()` (called every frame from
`animate()`) drives the walk cycle.

## Structure of the code (inside index.html)

- **HTML**: the gate/intro screen, the floating chapter title, side nav
  dots, the sound toggle, a screen-space vignette + grain overlay, and a
  tall invisible `#scroll-spacer` div that drives the camera as the user
  scrolls.
- **CSS**: fonts (headings in a warm red, `--red`), glassmorphic UI chrome,
  the vignette/grain overlay, responsive rules.
- **JS (`<script type="module">`)**:
  - `CH` — the eight chapters' titles, colors, fog and lighting data.
  - Sky, ground (with a tiled procedural detail texture), ocean — custom
    `ShaderMaterial`s.
  - City, forest, mountains, bridge, deer/chariot, characters — built once
    at load with plain meshes and `InstancedMesh` for repeated elements
    (trees, bridge stones, the vanara sena).
  - A cool-toned fill/rim light opposite the sun, so shadowed faces and
    buildings don't go flat black.
  - `posCurve` / `lookCurve` — the camera's flight path (`CatmullRomCurve3`)
    through all eight locales.
  - `animate()` — the render loop: maps scroll position to a point on the
    path, blends sky/fog/light colors between chapters, animates particles,
    flags, torches, the bridge's stone-by-stone reveal, and the characters.

## Customizing

- Change chapter text/colors/mood: edit the `CH` array near the top of the
  script.
- Change the camera's path: edit the `posPts` / `lookPts` arrays (two
  points per chapter: enter and exit).
- Adjust pacing: `#scroll-spacer` height is `800vh` (100vh × 8 chapters) —
  change that in the CSS to make the journey longer or shorter.
- Add or move a character: call `buildFigure({...})` with `skin`/`cloth`
  for a human, or `vanara:true` with `furColor` for a monkey character,
  then `addFigure(figure, walking, flying)` to place and register it for
  animation.
