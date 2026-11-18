# Hush Hollow

A kid-designed microphone game: be quiet and cute woodland animals come into a forest clearing; talk and they freeze and look sad; get loud and they run away.

## Files
- `index.html` — the whole game: one self-contained file (HTML + CSS + canvas JS), no build step, no dependencies.

## Running it
- Mic access needs a secure context. Serve the folder locally rather than opening it as a file:
  `python3 -m http.server 8000` then open http://localhost:8000/ (serves index.html) in Chrome.
- Test without a mic: append `?sim=0.05` (quiet), `?sim=0.45` (talking), `?sim=0.9` (loud).
  Comma-separated values switch every 5s, e.g. `?sim=0.05,0.45,0.9&all` (`&all` = Show All Animals).

## How it works (key spots in the script)
- Tuning constants at the top: `QUIET` (0.30) and `LOUD` (0.62) thresholds, `SPAWN_EVERY`, `QUIET_BEFORE_SPAWN`.
- `readLevel()` — mic RMS → dBFS → 0..1 (about -52 dBFS = 0, -14 dBFS = 1), scaled by the sensitivity slider (saved in localStorage).
- `SPECIES` — each animal: `move` (walk/hop/fly/hover), `h` height, `speed`, `tough` (how long it tolerates noise), `play` tricks, `face` anchors, `draw(p, a)` in local units facing right with feet at y=0.
- `makePen` / `drawCutout` — every shape is drawn 3 times (shadow, white edge, fill) for the paper-cutout look.
- `drawFace` — moods: happy, ok, sad (brows + tear), scared (wide eyes).
- `Animal` class — states: enter → play → flee; `fear` builds when loud, drains when quiet.
- `proj(x, z)` — fake-3D ground plane: x 0..1 across, z 0 (far) .. 1 (near).

## Design intent (from the kid)
- "If you talk, the animals don't come. But when they do come, you're not talking."
- Sound bar shows if you're too loud. Animals are cute and emotive: happy when quiet, sad when loud.
- Field with trees; the quieter it is, the more animals come, and they play around.
- A button that shows all the animals.
- Requested animals: parrot, hummingbird, hamster, iguana, bear, bobcat, squirrels (plus any woodland creatures).
- Flat Paper Mario style: paper characters hopping around a forest clearing.
