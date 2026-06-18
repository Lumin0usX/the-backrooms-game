# The Backrooms

> **Status: BETA** — this is an early build. Right now you can only walk around the procedural maze and experience atmosphere / horror events. The following are **not yet implemented**:
>
> - Inventory & Resource Management (items, batteries, sanity, stamina)
> - Progression System (objectives, levels, exits between Backrooms levels, save points beyond raw position)
>
> Expect rough edges. Feedback and PRs welcome.

A psychological-horror Backrooms game in a single self-contained HTML file. No dependencies, no assets — everything (textures, audio, world) is generated procedurally at runtime.

**[Play in browser](./index.html)** — just open `index.html` in any modern browser.

## Engine

- DDA raycaster with textured walls + per-row floor & ceiling casting
- 480×270 internal framebuffer, nearest-neighbour upscaled to fullscreen
- Infinite world built from 16×16 chunks with LRU eviction
- Maze-style room layout (5×5 cells with random doorways + occasional openings) seeded by value-noise / fBm
- Procedural wallpaper, wet carpet and ceiling-tile textures sampled from typed arrays
- Dynamic point lighting with inverse-square falloff, warm-yellow distance haze, vignette, scanlines and film grain
- Web Audio synthesised low hum (58/116 Hz + saw), fluorescent whine, filtered-noise footsteps, HRTF-spatialised disembodied steps, scream/jumpscare oscillators

## Controls

| Key | Action |
| --- | --- |
| `W` `A` `S` `D` / Arrows | Move |
| Mouse | Look |
| `F` | Toggle flashlight |
| `Esc` | Release pointer lock |

Position is persisted in `localStorage`.

## Horror events

Subtle, randomised, ambiguous. Cooldowns prevent overlap. Frequency slowly escalates the longer you walk.

- Disembodied footsteps directly behind you (HRTF) — stop the instant you turn
- Footsteps that match your own walking rhythm
- Fluorescent lights briefly flicker + buzz, then return to full
- A light turning off behind you, back on when you look
- Distant humanoid silhouettes that vanish before you can reach them
- Sub-second "glimpse" silhouettes during flickers
- Very rare long-distance shadow entities
- Spatial anomalies (a door that wasn't there, reverts when not observed)
- A single distant scream per session
- An extremely rare jumpscare (<1% per minute)

## Roadmap

Planned for future releases:

- **Inventory & Resource Management** — pickup-able items, flashlight battery, sanity / stamina meters
- **Progression System** — objectives, multi-level structure (Level 0 → Level 1 → …), discoverable exits, persistent run state

## License

[MIT](./LICENSE) &copy; 2026 Lumin0usX
