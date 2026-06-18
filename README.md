# The Backrooms

> **Status: BETA** — playable, but rough edges remain. Core loop now works: wander deep into the Backrooms, hunt down 3 items, unlock the door, descend into The Pools. 10 achievements track your progress, and a portable save code lets you carry your run anywhere.
>
> Still missing: sanity / stamina meters, additional Backrooms levels, audio mute UI toggle, settings menu.
>
> Feedback and PRs welcome.

A psychological-horror Backrooms game in a single self-contained HTML file. No dependencies, no assets — everything (textures, audio, world) is generated procedurally at runtime.

**[▶ Play online](https://lumin0usx.github.io/the-backrooms-game/)** — runs straight in the browser, no install.

Or download and open `index.html` locally in any modern browser.

## Levels

### Level 0 — The Backrooms
- Endless yellow-wallpaper maze, damp carpet, fluorescent buzz
- Three collectible items hidden **far from spawn** — each takes minutes of wandering through the maze to reach:
  - **Almond Water** (far east, ~75 tiles out)
  - **Old Battery** (far south, ~75 tiles out)
  - **Brass Key** (far southeast, deeper than both — ~110 tiles out)
- A locked **exit door** near spawn — pick up all 3 to unlock
- **Spare batteries** spawn rarely as glowing green drops, with denser drops the further you stray from spawn
- Walk into the door once unlocked → fade transition → descend to The Pools

### Level 1 — The Pools
- Endless abandoned indoor aquatic complex with **real elevation differences**
- Per-room layouts: dry corridors, half-flooded chambers, central pools with walkway perimeters, fully-flooded rooms
- Walk **up and down** between water (low) and dry walkway (raised by ~0.4 units)
- Visible step risers at every water/walkway boundary
- Wading in water slows movement to 66 %
- Cool-tinted dim fluorescent lighting, pale beige tile walls, teal water with subtle ripples
- Square support pillars, low and high ceilings, repetitive geometry — designed for liminal disorientation

## Inventory & Battery

- Press **`I`** to open the inventory panel
- Slots:
  - Almond Water (status)
  - Old Battery (status)
  - Brass Key (status)
  - **Spare Batteries** — count + `USE` button
  - **Flashlight** — live charge bar
- Flashlight drains its battery roughly every 2 minutes of use
- When the charge hits 0, the flashlight cuts out; install a spare battery to refill it
- A persistent HUD in the lower-left shows the charge bar + spare count
- Inventory state, battery count and charge all persist in `localStorage`

## Achievements

- Press **`J`** to open the achievements panel
- 10 hand-picked, hard-to-earn achievements. A Steam-style toast slides in from the right corner the moment you unlock one
- Examples (the easier handful):
  - **First Glimpse** — witness your first anomaly
  - **Three Trinkets** — collect all 3 Backrooms items
  - **Threshold** — descend into The Pools
- Harder ones include 5,000 walked tiles, 10 minutes in The Pools with the flashlight off, holding 8 spare batteries at once, and accumulating two hours of playtime
- Progress and unlocks persist in `localStorage` and travel with your save code

## Save Codes

- Press **`K`** to open the save-code panel
- Encodes your full run — position, level, inventory, batteries, achievements, walked / wading distance, anomaly count, picked-up battery drops — into a single portable string of the form `BR1-<base64>-<checksum>`
- **COPY** writes the current code to your clipboard; **REFRESH** regenerates it from the live state
- Paste any valid code into the load box and hit **LOAD** to replace the current save — useful for backing up before a risky push, hopping between devices, or sharing a deep run
- Codes carry a built-in checksum and version tag so a malformed paste is rejected instead of corrupting your save

## Controls

| Key | Action |
| --- | --- |
| `W` `A` `S` `D` / Arrows | Move |
| Mouse | Look |
| `F` | Toggle flashlight |
| `I` | Open / close inventory |
| `J` | Open / close achievements |
| `K` | Open / close save-code panel |
| `U` or `Enter` (in inventory) | Use a spare battery |
| `Esc` | Release pointer lock / close panel |

State is persisted in `localStorage` — position, current level, items collected, batteries, charge, achievements, walked / wading / playtime stats, and which battery drops you've already grabbed.

## Engine

- DDA raycaster with textured walls + per-row floor & ceiling casting (per-pixel water-vs-walkway texture switching in The Pools)
- 480×270 internal framebuffer, nearest-neighbour upscaled to fullscreen
- Infinite world built from 16×16 chunks with LRU eviction, separate cache per level
- Per-level world generators:
  - Backrooms: 5×5 maze cells with random doorways, deterministic battery drops keyed by chunk-relative coords (so a picked-up drop never respawns)
  - Pools: 16×16 room generator with corner pillars, door gaps, perimeter walkways and water sectors
- Player Z elevation with smooth-lerp transitions, camera horizon shift, and step-riser rendering during DDA
- Tight cone-projected flashlight (~28°) gated by battery charge
- Procedural textures: warm yellow wallpaper, wet carpet, ceiling tiles, beige pool wall tile, dry walkway, water surface
- Dynamic point lighting with soft inverse-square falloff (`I / (d² + 2)`), warm-yellow / cool-teal distance fog, vignette, scanlines, chromatic aberration, film grain
- Web Audio: low hum (58 / 116 Hz + saw), fluorescent whine, filtered-noise footsteps, HRTF-spatialised disembodied steps, scream / jumpscare oscillators
- Save codes are UTF-8 → base64 of a versioned JSON state plus an FNV-1a checksum suffix; loading validates the checksum before touching live state

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

- **Sanity / stamina meters** to round out the resource layer
- **More levels beyond The Pools** (Level 2 etc.) with discoverable exits
- **Settings menu** (volume, sensitivity, brightness)
- **Tile-accurate floor depth** in The Pools so distant water visually recedes below walkway level
- **More entity behaviours** for the horror events
- **More achievement tiers** as new levels and entity behaviours land

## License

[MIT](./LICENSE) &copy; 2026 Lumin0usX
