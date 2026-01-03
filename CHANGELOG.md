# Changelog

All notable changes to this project will be documented here.

## [1.1.0] - 2026-01-03
- Expanded the bird roster with varied point values and a rare dodo bonus.
- Nest now doubles the last bird score (not the dodo).
- Snake streak cheat now ends the game at 5000 points with a rain effect.
- Added a secret cheat code :shh:

## [1.0.1] - 2026-01-02
- Replaced the name popup with an inline name field.
- Reset PostHog identity when switching names or entering Guest mode.
- Track `leaderboard_opened` when the leaderboard link is clicked.

## [1.0.0] - 2026-01-02
- Added Guest mode and name controls for shared devices.
- Identify players only after they enter a name.
- Track person properties on `game_over` for named players.
- Added a Game Over overlay for clearer end-of-round feedback.
- Linked to the PostHog leaderboard.
