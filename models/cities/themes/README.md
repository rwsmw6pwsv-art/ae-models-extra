# #332 v8b — Instanced village / city kits (Option A)

True-size houses (king fits ~2.0 m door) packed to `VILLAGE_HOUSE_R` so `villageFit` scale is unchanged. Temples sit **in the house rings**. The pyramid/keep sits **alone** in the clearing at the `keepMetrics` courtyard footprint.

## Per theme (`instanced/<theme>/`)

| file | what |
|---|---|
| `house_hut.glb`, `house_booth.glb`, `house_hut_b.glb`, `house_booth_b.glb` | True-metre house variants, origin at ground centre, door faces **+Z**. LOD1 companions `*_lod1.glb` when cheap. |
| `temple.glb` | `dome-temples/out_sized` at **k_temple=k_house** → true-m H≈9.66 m (~2.5× house 3.8 m), under wood_keep_small 12.6 m and every pyramid. |
| `<layout>_base.glb` | Ground disc, ring paths, four gate roads (E/S/W/N), clearing plaza. **No houses.** |
| `<layout>_placements.json` | Transforms in **village GLB space** (same space `villageFit.ts` scales). |

Layouts: `village_small`, `village_large`, `city_small`, `city_large`.

## Units / fit space

- Space: village GLB (outermost house roof at `VILLAGE_HOUSE_R`).
- `village_small` outer **0.797**, `village_large` **0.812** (`repos/v3w/src/lib/game/villageFit.ts`).
- Cities use a larger disc (0.880 / 0.980) with the same true-size house `k` as `village_large`.
- `k_house` ≈ 0.0104–0.0106 so after wood-open `villageFit` the door is ≈ **2.0 m**.
- Placement `scale` is applied to the module (houses/temple authored at 1 kit-metre).

## How Engineer should draw

1. Load `<layout>_base.glb` once (roads + clearing + ground).
2. Load each `house_<variant>.glb` and `temple.glb`.
3. Read `<layout>_placements.json`.
4. **Always** draw every entry with `"always": true` (temples) — their spots stay free regardless of population N.
5. Draw the first **N** houses from `houses` (sorted by `order` ascending) as one `InstancedMesh` per variant.
6. Apply the **same** `villageFit(spec)` uniform scale + xz offset to the whole group (base + instances) that the game already uses for village GLBs.
7. Pyramid / keep: place alone at the origin of the clearing using `keepMetrics` (engine already does this). Preview scale in the header is for review only.

### Population (Michael Nov 9)

`order` is centre-outward by ring; within a ring, quadrants are interleaved so a partial outer ring fills evenly around all four roads (not one arc).

```
prefix(N) = temples(always) + houses.filter(order < N)
```

Header fields:
- `cumulative_by_ring`: per-ring counts and running totals
- `population.low_25pct` / `medium_60pct` / `full`

The game maps empire population → N; any prefix is a filled set of inner rings plus a partial next ring.

## Gate roads

Angles `[0, π/2, π, 3π/2]` = E/S/W/N from `BattleModeScreen.tsx` `wallRingLayout`.

## Instance counts (sand; other themes within ±2%)

| layout | houses | temples | rings | LOD0 instanced tris (houses+temples+base≈) |
|---|---:|---:|---:|---:|
| village_small | ~462 | 2 | 11 | see `qa/overlap.md` |
| village_large | ~456 | 3 | 11 | |
| city_small | ~491 | 3 | 11 | |
| city_large | ~693 | 4 | 13 | |

House hut ≈ 58 tris; booth similar. Full city_large ≈ 693×58 + 4×temple + base ≪ skinned unique mesh budget when instanced.

## QA

`qa/overlap.md` — overlaps 0, road hits 0, outside disc 0, temples-in-rings, counts, tris.

0 Meshy / not published / no repo or tag changes.

## Exact instance counts (this bake)

| theme | layout | houses | temples | rings | low N | med N |
|---|---|---:|---:|---:|---:|---:|
| sand | village_small | 462 | 2 | 11 | 115 | 277 |
| sand | village_large | 456 | 3 | 11 | 114 | 273 |
| sand | city_small | 491 | 3 | 11 | 122 | 294 |
| sand | city_large | 693 | 4 | 13 | 173 | 415 |
| jungle | village_small | 460 | 2 | 11 | 115 | 276 |
| jungle | village_large | 455 | 3 | 11 | 113 | 273 |
| jungle | city_small | 491 | 3 | 11 | 122 | 294 |
| jungle | city_large | 692 | 4 | 13 | 173 | 415 |
| snow | village_small | 462 | 2 | 11 | 115 | 277 |
| snow | village_large | 456 | 3 | 11 | 114 | 273 |
| snow | city_small | 491 | 3 | 11 | 122 | 294 |
| snow | city_large | 693 | 4 | 13 | 173 | 415 |
| forest | village_small | 462 | 2 | 11 | 115 | 277 |
| forest | village_large | 456 | 3 | 11 | 114 | 273 |
| forest | city_small | 491 | 3 | 11 | 122 | 294 |
| forest | city_large | 693 | 4 | 13 | 173 | 415 |


## Wall world size (v8b)

After wood-open `villageFit`, `village_small` and `village_large` walls both map to **REACH_WOOD = 75** world units (same wall size in battle). Large uses a bigger OUTER in GLB space (0.812 vs 0.797) with a denser pack so **n_houses(large) > n_houses(small)** and large reads denser. Cities use still-larger discs and clearly more houses than villages.

## QA

`python3 qa/assert_v8b.py` — order ring non-decreasing, cum counts, large>small, cities>villages, temple scale, 0 overlaps/road/outside.
