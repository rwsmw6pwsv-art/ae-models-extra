# ae-models-extra v14 — #136 battlefield rocks, banners and sky/fog

Michael approved the #136 art look and, on Sat Nov 28 2026 (11:42 Athens, relayed by COO), OK'd publishing with these placement rules:

1. No rocks inside the city walls or the keep area.
2. Sparse, scattered rocks outside the city.
3. Sparse, scattered rocks in Village Raid battles too.

Nothing in this tag overwrites live files. v8 stays reserved for #142. Existing tags are untouched.

## Paths (new)

- `models/terrain/battle_props/rocks/ae_battle_rock_{small,boulder,cluster}_lod{0,1}.glb`
- `models/terrain/battle_props/banners/ae_battle_banner_{atk,def}_lod{0,1}.glb`
- `models/terrain/battle_props/atlas/rock_<biome>.webp` (10 biomes) and `banner_atlas.webp`
- `models/terrain/battle_props/placements.json` — machine-readable rules + city_siege and village_raid sample instances
- `models/terrain/battle_props/ENGINEER-NOTES.md` — file list, tris, sizes, loader, skyfog usage, rules
- `models/terrain/skyfog/skyfog.json` — per-biome fog/sky from the approved #338 backdrop manifest (colours not re-authored)
- `models/terrain/skyfog/skyfog_swatches.png`

## Art

Shared procedural meshes, Nov 10 look, 0 Meshy.

| Asset | LOD0 tris / native height | LOD1 tris |
|---|---|---|
| rock small | 144 / 2.43 m | 40 |
| rock boulder | 144 / 4.30 m | 40 |
| rock cluster | 224 / 5.76 m | 56 |
| banner atk/def | 220 / 13.24 m | 100 |

Instance the three rock meshes and swap `atlas/rock_<biome>.webp` as the albedo (flipY=false, metallic 0, roughness 0.95). Banners: double-sided cloth, atlas already holds both cloths. Sit on y=0. Quality tier (#101): low can drop small rocks and use LOD1 only.

Replace live `BATTLE_HAZE` / `BATTLE_SKY` with `skyfog.json` per biome. Official backdrops stay the high-tier sky; the hex gradient is the low-tier fallback. Do not invent a second sky.

## Placement rules (values in placements.json → rules)

Taken from the game repo (read only): outer wall 198 m, inner 162 m, keep at (0.5, −2.8). Seed 13628.

**City / siege:** exclusion radius 206.4 m (198 + 2.4 wall half + 6 m clearance); keep exclusion 77.81 m; charge lane |x| ≤ 28 m, z from 198 to 408; place in the annulus out to field radius 500 m; density 0.022 / 100 m²; min spacing 36 m; 143 sample rocks. Banners are a fixed layout: attacker (−72, 320) and (78, 312); defender (−88, 220) and (86, 216), just outside the south wall, off the charge lane.

**Village Raid:** 2400 × 1600 m map; hamlet radius 60+16 m; river, lake, roads, and 24 m entry/extract pads kept clear; density 0.012 / 100 m²; min spacing 40 m; 461 sample rocks; no banners.

Engineer can regenerate from the rule fields with a different seed; the presets are one sample.

## Wiring

See `models/terrain/battle_props/ENGINEER-NOTES.md`. Integration into the game still follows the usual Engineer pass; this tag only publishes the art pack.
