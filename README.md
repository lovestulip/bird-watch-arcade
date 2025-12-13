# Bird Watch Arcade 🦩

A lightweight arcade-style game built to explore **PostHog product analytics**.

Players tap birds to score points, use nests strategically, and avoid snakes.
Flamingos are high-value targets, while snakes reset your score.

This project is intentionally simple so the focus stays on **event design,
instrumentation, and analysis** rather than frameworks.

---

## 🎯 Why I Built This

I wanted a **real, hands-on project** to deeply understand PostHog beyond docs:

- Designing meaningful product events
- Understanding how autocapture + custom events interact
- Verifying analytics locally and in production
- Experimenting with funnels, retention, and behavior

This game acts as a controlled environment for learning analytics properly.

---

## 🎮 Gameplay Rules

- 🦩 **Flamingo** — highest score  
- 🐦 **Other birds** — lower score  
- 🪺 **Nest** — doubles the previous bird’s points  
- 🐍 **Snake** — resets your score to zero  

Each game session lasts **30 seconds**.

---

## 📊 Instrumented Events

The game explicitly tracks the following events using `posthog.capture()`:

- `game_start`
- `tap_bird`  
  - `bird_type`
  - `base_points`
  - `score_after`
  - `time_left`
- `tap_nest`
- `tap_snake`
- `game_over`
  - `final_score`
  - `duration_s`

These events are **manually defined in the game logic** and are separate from
PostHog’s default autocapture events (pageviews, web vitals, etc.).

---

## 🌐 Live Versions

- **GitHub Pages (standalone game)**  
  https://lovestulip.github.io/bird-watch-arcade/

- **Embedded on WordPress**  
  https://thebabyflamingo.wordpress.com/

Both versions send events to the same PostHog project, allowing comparison
between embedded vs standalone usage.

---

## 🛠 Local Development

To run the game locally:

```bash
python3 -m http.server 5173
```
Then open
`http://localhost:5173`

To manually test tracking in browser console:

`posthog.capture('debug_test_event', { source: 'localhost' })`

---
Have fun — and watch out for snakes 🐍
