# Bird Watch Arcade 🦩

A lightweight arcade-style game built to explore **PostHog product analytics**.

Players tap birds to score points, use nests strategically, and avoid snakes.
Flamingos are high-value targets, while snakes reset your score.
There's also a little secret in the game :shh:

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
- 🕊️🦆🦅🦉🦜🦢🪿🐦‍⬛ **Other birds** — varied point values  
- 🦤 **Dodo** — rare, triples your last bird points, starts a 10s fever, and adds +5s  
- 🪺 **Nest** — doubles your last bird’s points (not the dodo); during fever it’s x5  
- 🔥 **Fever** — lasts 10s, birds are 1.5x and nests are x5  
- 🐍 **Snake** — resets your score to zero  

Each game session lasts **30 seconds**.

---

## 📊 Instrumented Events

The game explicitly tracks the following events using `posthog.capture()`:

- `game_start`
  - `duration_s`
- `tap_bird`
  - `bird`
  - `score_after`
  - `fever_active`
- `tap_nest`
  - `points`
  - `score_after`
  - `fever_active`
- `tap_snake`
  - `score_after`
- `tap_dodo`
  - `points`
  - `score_after`
  - `time_bonus_s`
- `fever_started`
  - `score_at_start`
  - `time_left`
- `fever_ended`
  - `score_at_end`
  - `time_left`
- `snake_cheat_unlocked`
  - `score_after`
- `game_over`
  - `final_score`
  - `reason`
  - `$set` (only when a name is set)
    - `last_score`
    - `high_score`
    - `games_played_count`
- `game_reset`
  - `score_before_reset`
  - `time_left`
- `leaderboard_opened`
  - `source`

These events are **manually defined in the game logic** and are separate from
PostHog’s default autocapture events (pageviews, web vitals, clicks, etc.).

Autocapture and pageviews are enabled.

---

## 🚩 Feature Flags

- `ff_audio_mode` — enables subtle tap sounds and a gentle game-over tune.

---

## 🧑‍💻 Names, Profiles, and Privacy

- Players are **anonymous by default**.
- If a player enters a name, we call `posthog.identify()` once and start tracking
  person properties for that player.
- If no name is set, we still capture events but **do not** create a person
  profile or attach person properties.
- Names are entered via the inline UI (no popup prompt).
- Guest mode clears the stored name and resets PostHog identity on this device.
- Switching to a new name also resets identity, so each player gets a separate
  person profile.
  This avoids mixing multiple players on shared devices and keeps profiles
  intentionally scoped to a single person.

---

## 🏆 Leaderboard

Open the leaderboard in PostHog (also linked from the in-game UI):
https://us.posthog.com/embedded/xf1YTaMVIL5JKMw87qQl4rki_VU9qA

The SQL insight ranks players by max `game_over.final_score` over the last 90 days.
It shows display name (or a Guest alias), highest score, score timestamp, and city
from GeoIP for context.

---

## 🌐 Live Versions

- **GitHub Pages (standalone game)**  
  https://lovestulip.github.io/bird-watch-arcade/

- **Embedded on WordPress**  
  https://thebabyflamingo.wpcomstaging.com/2025/12/12/bird-watch-arcade/

Both versions send events to the same PostHog project, allowing comparison
between embedded vs standalone usage.

---

## 📓 Changelog

See [CHANGELOG.md](CHANGELOG.md).

---

## 🛠 Local Development

To run the game locally:

```bash
python3 -m http.server 5173
```
Then open
`http://localhost:5173`

---

## 🧪 Debugging Analytics with a Test Event

Send a manual test event from the browser console to confirm PostHog is wired
up correctly.

**How to send it**

1. Open the game in your browser.
2. Open Developer Tools → Console.
3. Run:

```js
posthog.capture('debug_test_event', { from: 'console' })
```

**How to find it in PostHog**

1. Go to PostHog → Activity.
2. In the event search bar, type `debug_test_event`.
3. Select the event to view its properties (you should see `from: "console"`).

Seeing that event confirms PostHog initialization works, the network request
is not blocked, and your project is receiving events from this build.

---
Have fun — and watch out for snakes 🐍
