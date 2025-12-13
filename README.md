# Bird Watch Arcade 🦩

A lightweight arcade-style game built to explore PostHog product analytics.

Players tap birds to score points, build combos, and chase multipliers.
Rare flamingos are high value, while snakes reset combos.

## Why I built this
I wanted a small, real project to deeply understand PostHog beyond docs:
event design, funnels, behavioral analysis, and iteration based on data.

## Instrumented events
- `game_start`
- `tap_bird` (bird type, points, multiplier, score, combo)
- `combo_milestone`
- `game_over` (final score, max combo, duration)

## Tech
- Vanilla HTML / CSS / JS
- Single-file app (no build step)
- PostHog JavaScript snippet
