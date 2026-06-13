# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Game

Open `voxelcraft.html` directly in a browser — no build step, no package manager, no server required. Three.js r128 is loaded via CDN. For quick iteration, use a local file server to avoid CORS issues with future asset loading:

```
npx serve .
# or
python -m http.server
```

## Architecture

The entire game lives in a single file: `voxelcraft.html` (~2,670 lines of vanilla JS + inline CSS + HTML). There are no modules, bundlers, or transpilation steps.

### Major Systems (by line range)

| System | Lines |
|---|---|
| Block registry & inventory data | 1–525 |
| World generation (chunks, terrain, biomes, caves, trees) | 525–778 |
| Rendering pipeline & held item models | 780–1239 |
| Player physics, movement, AABB collision | 1241–1546 |
| Game state machine (pregame / playing / gameover) | 1548–1670 |
| Zombie AI, horde wave management | 1671–2039 |
| Item drops & pickups | 2042–2109 |
| Combat (melee, bow, bombs, turrets) | 1371–1475, 2177–2261 |
| UI (inventory, crafting, HUD) | 2110–2400 |
| Particle effects & visual feedback | 2399–2452 |
| Main game loop | 2549–2660 |
| Initialization | 2662–2667 |

### Rendering

Two-pass rendering using Three.js WebGL:
- **Pass 1 (layer 0):** World geometry — face-culled meshes with vertex color lighting
- **Pass 2 (layer 1):** Held item — rendered on top via depth-clear before draw

Lighting is baked per-vertex at mesh generation time using two sources: torch light (9-block radius falloff) and sky light (day/night tint). Meshes must be regenerated (`buildChunk`) when blocks or torch positions change.

### World Structure

- Chunks are 16×16×64. Only chunks within the render distance circle are loaded; beyond that they are unloaded and their Three.js meshes disposed.
- Three biomes: Forest (start), Iron Mountains (east, unlocks after night 1), Sulfur Desert (west, unlocks after night 2).
- Block IDs are plain integers. The `blockDefs` array at the top of the file is the canonical registry.

### Game Loop

The `loop()` function (line ~2549) runs via `requestAnimationFrame`. Frame delta is capped at 50ms. Update order: player → held item animation → day/night → zombie AI → item drops → projectiles → turrets → leaf decay → particles → chunk streaming (6ms budget) → render.

### Key Global State

- `world` — flat `Int8Array` mapping block positions to block IDs
- `chunks` — `Map` of loaded chunk meshes keyed by `"cx,cz"`
- `player` — position, velocity, health, inventory, hotbar selection
- `zombies`, `drops`, `projectiles`, `turrets` — arrays of active entities
- `gameState` — `"pregame"` | `"playing"` | `"gameover"`
- `night` — current night number (drives difficulty scaling)

### Combat & Difficulty Scaling

Zombie count per wave: `6 + 4 * night`. Spawn cap: `min(8 + 2 * night, 22)`. New zombie types unlock on specific nights (Runner: 2, Brute: 3, Spitter: 4). Zombie HP and damage also scale with `night`.

## Controls (for testing)

- `WASD` — move, `Space` — jump, `Shift` — sprint
- Left-click — attack/break, Right-click — place/use
- `E` — inventory, `1–8` / scroll — hotbar
- `N` — skip to next night/phase (useful for testing later waves)
- `R` — restart run
- `[ ]` — decrease/increase render distance
