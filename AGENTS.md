# Asteroids — Agent Guide

## Project

Single-file HTML5 Canvas game. No build step, no dependencies, no bundler.

- `index.html` — shell, loads `game.js`
- `game.js` — all game logic (classes: Ship, Asteroid, Bullet, Particle)
- Canvas is fixed 800×600

## Run

Open `index.html` in browser directly, or:

```bash
npx serve .
```

## Code conventions

- `'use strict';` at top of `game.js`
- All game state is module-scoped (no exports, no classes used as modules)
- Input via `keys` object (hold) and `justPressed` (one-shot); use `pressed(code)` helper for single-press actions
- Canvas context `ctx` is a module-level constant — all draw methods use it directly
- Coordinate wrapping: use `wrap(value, max)` helper for toroidal space
- Language: README and UI text are in Spanish

## Gotchas

- `dt` is clamped to 0.05s max in the loop to prevent physics explosions on tab-refocus
- Ship has 3s invincibility on spawn (blinking effect via `Math.floor(inv * 8) % 2`)
- Asteroid sizes are 1 (small), 2 (medium), 3 (large) — indexed into `RADII`, `SPEEDS`, `POINTS` arrays
- No tests, no linting, no formatter — verify changes manually in browser
- Ship skins live in the `SKINS` registry (data-driven: `body`, `stroke`, `fill`, `glow`, `thrust`); add new entries there. `currentSkinIndex` persists via `localStorage['asteroids.skin']` and cycles with `S`
- Power-ups are data-driven: register new types in `POWERUP_RADII` (radius per type), add a spawn branch in `spawnPowerUp()`, an icon branch in `PowerUp.draw`, a pickup branch in `update()`, and a HUD row via `drawPowerBar(label, timer, color, duration, row)`. Timers live in `Ship` (e.g. `shieldTimer`, `tripleTimer`) and decrement in `Ship.update`
