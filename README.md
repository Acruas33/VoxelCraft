# VoxelCraft

A voxel roguelike horde-defense game that runs in the browser. Mine, craft, and build by day; protect your **Base Core** from waves of zombies by night. Each dawn you survive unlocks new lands — and each night the horde grows larger and meaner.

Built as a single self-contained HTML file with vanilla JavaScript and [Three.js](https://threejs.org/). No build step, no dependencies to install.

## Play

Just open `voxelcraft.html` in a modern browser.

For the smoothest experience (and to avoid any browser file restrictions), serve it locally instead:

```bash
npx serve .
# or
python -m http.server
```

Then open the printed `localhost` URL and click the canvas to start.

## How to play

Place your **Base Core** to start the clock. From there it's a loop of **build by day, survive by night**:

- **Gather** wood, stone, coal, iron, and sulfur from the world.
- **Craft** weapons, torches, walls, and automated defenses.
- **Fortify** the area around your core before nightfall.
- **Survive** the wave. Each dawn you make it through opens up new territory and ramps the difficulty.

If your core is destroyed, the run is over — you're scored on the number of nights you survived.

### The world

Three biomes sit in a strip around your spawn, unlocked by surviving:

| Biome | Location | Unlocks | Offers |
|---|---|---|---|
| **Forest** | spawn | start | wood, coal, the basics |
| **Iron Mountains** | east | after night 1 | iron (for swords & turrets) + walkable ore tunnels |
| **Sulfur Desert** | west | after night 2 | sulfur (for bombs) + cacti |

### The horde

New zombie types join the rotation as the nights stack up:

| Night | Type | Behavior |
|---|---|---|
| 1 | **Walker** | baseline shambler, digs through walls |
| 2 | **Runner** | fast and fragile |
| 3 | **Brute** | slow, tanky, smashes walls quickly |
| 4 | **Spitter** | ranged acid that etches your walls |
| 5 | **Bomber** | charges the core and self-destructs to breach defenses |

Zombies share the work breaking down your walls, so sturdier blocks (brick > stone > wood) buy you more time. Walls slowly repair themselves between attacks. Daylight burns any zombie caught under open sky.

## Crafting

| Item | Recipe |
|---|---|
| Planks | 1 Log |
| Sticks | 2 Planks |
| Torches | 1 Stick + 1 Coal |
| Wood Sword | 2 Planks + 1 Stick |
| Stone Sword | 2 Stone + 1 Stick |
| Iron Sword | 2 Iron + 1 Stick |
| Bow | 3 Sticks + 2 Leaves |
| Arrows | 1 Stick + 1 Stone |
| Bombs | 2 Sulfur + 1 Coal |
| Brick | 2 Stone |
| **Turret** | 1 Bow + 2 Planks + 2 Iron |
| **Bomb Turret** | 3 Bombs + 2 Iron + 2 Planks |

**Turrets** auto-fire bolts at the nearest zombie. **Bomb turrets** lob arcing splash bombs at the densest cluster and arc over your own walls without damaging them.

## Controls

| Key | Action |
|---|---|
| `W A S D` | Move |
| `Mouse` | Look |
| `Space` | Jump / swim up |
| `Shift` | Sprint |
| `Left click` | Break block / attack |
| `Right click` | Place block / eat / use item |
| `E` | Inventory & crafting |
| `1`–`8` / scroll | Select hotbar slot |
| `N` | Skip to nightfall / cleared night → dawn |
| `[` `]` | Decrease / increase render distance |
| `R` | Restart run |
| `` ` `` | Dev / creative menu |
| `Esc` | Release the mouse |

## Dev / creative menu

Press `` ` `` to open a built-in testing panel:

- **Creative mode** — free crafting, invulnerability, and flight (double-tap `Space`)
- **Give items** — grab any block, item, or mob **spawn egg**
- **Spawn enemies** — drop any zombie type on demand
- **Toggles** — mob aggro, freeze time, base invincibility, block regeneration
- **Skip nights** — jump straight into a later night's wave

## Tech notes

- Single file: `voxelcraft.html` (markup, styles, and game code together)
- Chunked voxel world with face-culled meshing and vertex-color lighting
- Three.js r128 loaded from a CDN
- All art (block textures, item icons, mobs) is generated procedurally — no asset files

See [`CLAUDE.md`](CLAUDE.md) for an architecture overview.
