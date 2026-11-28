# #136 battlefield rocks, banners and sky/fog — Engineer notes

Pack: `/workspace/assets/issue-136/pack/`
Issue: #136. 0 Meshy. Art look is the Nov 10 draft Michael approved. Placement follows his Nov 28 rules. Coordinates are game-world metres, Y-up, city centre at the origin.

Do not overwrite live files. Suggested repo paths (new, additive):

- `models/terrain/battle_props/rocks/`
- `models/terrain/battle_props/banners/`
- `models/terrain/battle_props/atlas/`
- `models/terrain/battle_props/placements.json`
- `models/terrain/skyfog/skyfog.json`

## Files

| File | Tris | Size | Notes |
|---|---|---|---|
| `rocks/ae_battle_rock_small_lod0.glb` | 144 | 93,172 B | native height 2.43 m; default steppe albedo embedded; roughness 0.95, metal 0 |
| `rocks/ae_battle_rock_small_lod1.glb` | 40 | 82,564 B | native height LOD1; default steppe albedo embedded; roughness 0.95, metal 0 |
| `rocks/ae_battle_rock_boulder_lod0.glb` | 144 | 93,176 B | native height 4.30 m; default steppe albedo embedded; roughness 0.95, metal 0 |
| `rocks/ae_battle_rock_boulder_lod1.glb` | 40 | 82,564 B | native height LOD1; default steppe albedo embedded; roughness 0.95, metal 0 |
| `rocks/ae_battle_rock_cluster_lod0.glb` | 224 | 101,340 B | native height 5.76 m; default steppe albedo embedded; roughness 0.95, metal 0 |
| `rocks/ae_battle_rock_cluster_lod1.glb` | 56 | 84,200 B | native height LOD1; default steppe albedo embedded; roughness 0.95, metal 0 |
| `banners/ae_battle_banner_atk_lod0.glb` | 220 | 39,048 B | 13.24 m pole, cloth 3.4×5.2 m, double-sided; roughness 0.82, metal 0 |
| `banners/ae_battle_banner_atk_lod1.glb` | 100 | 26,804 B | 13.24 m pole, cloth 3.4×5.2 m, double-sided; roughness 0.82, metal 0 |
| `banners/ae_battle_banner_def_lod0.glb` | 220 | 39,048 B | 13.24 m pole, cloth 3.4×5.2 m, double-sided; roughness 0.82, metal 0 |
| `banners/ae_battle_banner_def_lod1.glb` | 100 | 26,804 B | 13.24 m pole, cloth 3.4×5.2 m, double-sided; roughness 0.82, metal 0 |
| `atlas/rock_desert.webp` | — | 84,478 B | 512² albedo override for all three rock meshes |
| `atlas/rock_savanna.webp` | — | 97,666 B | 512² albedo override for all three rock meshes |
| `atlas/rock_mediterranean.webp` | — | 95,800 B | 512² albedo override for all three rock meshes |
| `atlas/rock_jungle.webp` | — | 137,762 B | 512² albedo override for all three rock meshes |
| `atlas/rock_forest.webp` | — | 92,546 B | 512² albedo override for all three rock meshes |
| `atlas/rock_grass.webp` | — | 110,004 B | 512² albedo override for all three rock meshes |
| `atlas/rock_steppe.webp` | — | 76,940 B | 512² albedo override for all three rock meshes |
| `atlas/rock_mountain.webp` | — | 138,462 B | 512² albedo override for all three rock meshes |
| `atlas/rock_ice.webp` | — | 76,576 B | 512² albedo override for all three rock meshes |
| `atlas/rock_tundra.webp` | — | 77,640 B | 512² albedo override for all three rock meshes |
| `atlas/banner_atlas.webp` | — | 15,146 B | shared 512 atlas (wood, bronze, atk cloth, def cloth) |
| `skyfog/skyfog.json` | — | 4,796 B | per-biome fog/sky from approved #338 v7/v13 backdrops/manifest.json |
| `placements.json` | — | 118,967 B | rules + city_siege and village_raid sample instances |

Art files (GLB + WebP + skyfog, no placements): **1.61 MB**. With placements.json: **1.73 MB**. Under the +3–4 MB budget. No KTX2 in this pack; encode later if wanted.

## Loader

- Sit every mesh on y=0; they are already grounded.
- Instance the three rock meshes. Swap `atlas/rock_<biome>.webp` (or a later KTX2) as the albedo. One material, metallic 0, roughness 0.95. Load textures with flipY=false (UVs are authored for that).
- Two banner files, or one mesh with a cloth UV swap. The atlas already holds both cloths (atk terracotta with a gold chevron, def slate). Cloth is double-sided; do not also emit a back face.
- Quality tier (#101): low can drop small rocks and use LOD1 only.
- Kings are unrelated; this pack has no unit armour.

## Sky and fog

Replace the live single `BATTLE_HAZE` (`#e4c29a`, near 720, far 1760) and `BATTLE_SKY` with `skyfog/skyfog.json` per TerrainId / biome. Colours are the approved #338 samples (`fog_hex`, `sky_top_hex`, `horizon_land_hex`, `ground_disc_hex`). Do not invent a second sky. Official v7/v10 backdrops stay the high-tier sky. The hex gradient `sky_top_hex → fog_hex → ground_disc_hex` is the low-tier fallback. `fog_near` / `fog_far` are this draft's density suggestion.

## Placement rules (Michael, Nov 28)

(a) No rocks inside the city walls or the keep area.
(b) Sparse, scattered rocks outside the city.
(c) Sparse, scattered rocks in Village Raid battles too.

Machine-readable fields are in `placements.json` → `rules`. Generate in code from those values if you need a different seed; `presets` are one sample at seed **13628**.

Taken from the game repo (read only): `battle/constants.ts`, `siege.ts`, `city.tsx` keepMetrics, `villageRaid.ts`.

### City / siege preset

| Rule | Value |
|---|---|
| Seed | 13628 |
| Outer wall radius | 198 m |
| Inner wall radius | 162 m |
| City exclusion radius | 206.4 m (198 + 2.4 wall half + 6 m rock clearance) |
| Keep centre | x = 0.5 m, z = −2.8 m |
| Keep courtyard radius | 51.84 m (inner 162 × 0.32) |
| Keep exclusion radius | 77.81 m (courtyard × √2 + 4.5) |
| Charge lane | \|x\| ≤ 28 m, z from 198 m (south gate) to 408 m (HOLD_R1) |
| Place in | annulus [206.4, 500] m, outside the charge lane |
| Density | 0.022 per 100 m² |
| Min spacing | 36 m |
| Sample count | 143 rocks (55 small, 47 boulder, 41 cluster) |

Banners are a fixed layout, not randomised, same role as the Nov 10 review (cloth faces the south / opening camera, ry ≈ π):

- attacker: (−72, 320) and (78, 312)
- defender: (−88, 220) and (86, 216), just outside the south wall, off the charge lane

### Village Raid preset

| Rule | Value |
|---|---|
| Seed | 13628 (+17 for the raid dart stream) |
| World | 2400 × 1600 m |
| Hamlet exclusion | village radius 60 m + 16 m pad (West 340,760; South 520,1360; North 1100,300; East 1960,740) |
| Also kept clear | river (half 36+18 m), lake at (1320, 170) rx 175 rz 115, roads (half 16 m), 24 m pads at entry (200, 1420) and extract (2180, 200) |
| Density | 0.012 per 100 m² |
| Min spacing | 40 m |
| Sample count | 461 rocks |
| Banners | none |

## Verify

No city-siege sample rock has `hypot(x,z) < 206.4`, `hypot(x−0.5, z+2.8) < 77.81`, or sits in the charge lane. No Village Raid sample rock sits in a hamlet, river, lake, road or entry/extract pad.
