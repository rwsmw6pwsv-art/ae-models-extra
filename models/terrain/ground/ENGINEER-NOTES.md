# Ground rework v5 — engineer notes (jungle + mediterranean)

Issue #338, unpublished. 0 credits. CC0 textures only (Poly Haven, ambientCG).
Look approval still required; this pack is not integrated and not published.
v1–v3 are archived at `ground_v1/`–`ground_v3/` and `review/ground/v1/`–`v3/`.
v4 jungle and mediterranean are archived at `ground_v4/` and `review/ground/v4/`.

The other eight biomes (desert, savanna, forest, grass, steppe, mountain, ice, tundra) were **look-approved** on the v4 pack and are frozen byte-identical in `ground/approved/` (textures, manifest entries, TEST backdrops, and a copy of the v4 ENGINEER-NOTES). That freeze is not modified. `backdrops/approved/` was not touched.

## What changed in v5

Two reworks, one sheet (`review/ground/sheet_rework.jpg`, title **Ground rework v5: jungle + mediterranean, unpublished**). No tone retune on the eight frozen biomes. Shader recipe unchanged (per-biome uniforms only).

### 1. Jungle — whole-disc brighten

Michael: “Jungle needs rework it is too dark.” The v4 floor was dark leaf litter (baked albedo mean L ≈ 56, `forest_leaves_03`, shader `albedo_gain` already 2.0) and read as nearly black in-engine (play-area L ≈ 15 vs the photo’s sunlit canopy foot L ≈ 75). Fade-ring lerp alone could not fix that.

v5 replaces the albedo with a **midtone-lifted, olive-graded** `forest_leaves_03` (sunlit leaf litter with green-brown variation, lighter leaves, darker pockets kept) and keeps olive-graded `forrest_ground_01` as mossy soil patches. A different CC0 source was considered (`forrest_ground_01` as the main albedo); it read as a pale grassy lawn under the warm key, so leaf litter stayed the lead texture, just lifted and graded so it does not go beige or washed-out under key `#ffc48a`.

| | v4 | v5 |
|--|----|----|
| albedo | `forest_leaves_03` unlifted, L mean 56, p10 23 | same source, midtone-lifted + olive grade, L mean 110, p10 64, p90 157 |
| patch | `forrest_ground_01` | same, mild olive grade |
| tint | `#e8f4c8` | `#e4f0cc` |
| albedo_gain | 2.00 | 1.36 |
| far_lift | 1.10 | 1.10 |
| foot_color / lerp | `#4a5535` @ 0.80 | `#5a7348` @ 0.70 |
| pack size | 300 KB | 297 KB (≤300 KB) |

Raised-camera luma: play 15 → 45; mid-fade 28 → 58; photo foot stays 75. Battle mid-fade 51.5 vs foot 55.5 (almost meets). The disc now reads as sunlit tropical leaf litter, not a dark pit. A residual brightness and hue step remains at the raised join (olive-brown litter vs the photo’s lime canopy — different materials). It is much milder than v4’s dark cliff, not gone.

Sources: [forest_leaves_03](https://polyhaven.com/a/forest_leaves_03), [forrest_ground_01](https://polyhaven.com/a/forrest_ground_01), both CC0, Poly Haven.

### 2. Mediterranean — remove the flat grey haze band

Michael: “Grey band also needs fixing.” The v4 local fog mask hid the hillside houses by painting official-style fog through the unmasked band. That read as a flat grey rectangle in the raised view.

v5 keeps the 10° unmask and the locked v2 mediterranean ground textures. The TEST backdrop land (below `horizon_v` only) was rebuilt from the v3 unmasked photo:

- **Left inhabited hills:** the village is replaced by one continuous stretch of building-free forested hillside from the same equirect (x ≈ 1450–2550, same y so the photo’s atmosphere is kept), with a wide cross-dissolve and a flipped second stretch so it does not picket-fence. The real meadow foot is kept; the field house, farmyard and pole are tightly filled from the open meadow to their left.
- **Right treeline:** houses only (the olive field is kept). Each cluster is Poisson-cloned from the neighbouring olive/tree slope at the same y. A first attempt that tiled or scaled a wide field patch left a rectangular inset and a mirrored ridge; that was rejected.
- Sky above `horizon_v` is restored from the approved backdrop and is **source-array byte-identical** (`sky_diff_px=0`). On-disk sky MSE ≈ 0.28 is the usual lossy q=90 drift. File size 112 KB (4k) / 45 KB (2k).

Builder: `ground/tools/rebuild_med_v5.py`. Verified at full size on the default battle and raised frames and on all 4 yaws × both cameras (16 frames). The grey bank is gone at every one of those views, including the yaw that used to be a full-width grey bar. No building, house, road or fence is visible in the unmasked band.

A few pale structures sit on hills that already rise *above* `horizon_v` in the approved photo (visible in v4 above the grey bar too). Those are in the approved sky, were left untouched, and stay source-array identical. There is a mild tree-type change at the official horizon on the inhabited left (the fill is mid-panorama forest under mixed autumn hills that already poke into the approved sky). The photo’s own valley mist at other yaws is real atmosphere, not a painted bank.

### Review (honest, not a self-pass)

I viewed the v5 battle and raised frames at 1280×720, the 2× join crops, and the 4-yaw strips for both cameras. I do not pass this for Michael or the COO.

- **jungle** — the floor is a clearly brighter sunlit leaf litter, with mid-tones and lighter green-brown patches, and it does not go beige or washed-out under the warm key. Play-area luma is about 3× v4. The raised join is much closer than v4 but a residual olive-vs-lime brightness/hue step is still readable against the sunlit canopy foot. Battle join is close. Pack 297 KB.
- **mediterranean** — the flat grey band is gone at all 4 yaws, both cameras. The band now reads as forested hillside and meadow fading into the photo’s own haze. Ground disc textures are unchanged. A few already-approved houses remain on hills above `horizon_v`. Mild tree-type change at the horizon on the left inhabited yaw, as above.

The eight frozen biomes were not changed (textures still md5-identical to `ground/approved/`). Remaining honest gaps on those eight are unchanged from v4 (grass tone step, mountain photo haze). Nothing is published. I do not self-pass.

---

# Ground pass v4 — engineer notes (archive; jungle and mediterranean later reworked as v5)

Issue #338, unpublished. 0 credits. CC0 textures only (Poly Haven, ambientCG).
Look approval still required; this pack is not integrated and not published.
v1 is archived at `ground_v1/` and `review/ground/v1/`.
v2 is archived at `ground_v2/` and `review/ground/v2/`.
v3 is archived at `ground_v3/` and `review/ground/v3/`.

Textures and colours remain **locked from v2** (md5-identical to `ground_v2/`). This pass is polish on the v3 structural join. `backdrops/approved/` was not touched. No further tone or foot-lerp tuning after the first v4 read.

## What changed after the first v4 read (object-mask + size)

The first v4 still showed mediterranean hillside houses and the forest farmstead in the raised frames. Those objects sit *inside* the official `keep_land` sliver (and in the official gradual fog below it). The approved photos themselves still contain them; v2 hid them only because the ground disc was opaque. Shrinking unmask depth enough to cover them would also collapse the 10° photo-land band that makes the v3 join work, so every biome **keeps a 10° unmask**. Objects in the band are hidden with a **local approved-style fog mask** over the building / road / fence only, not by giving up the join.

Test backdrops were also re-encoded as **lossy WebP quality 90** so each 4k file is well under the 2 MB cap and the 600 KB ideal (76–242 KB). On-disk sky cannot be byte-identical under lossy (sky MSE 0.12–1.16, same class of drift as v3 q=88). The contract is source-array sky identity, verified before encode.

Builder: `ground/tools/rebuild_test_v4b.py`. Shader, fade recipe, lighting, and locked v2 textures were not changed. Sheet title stays **Ground pass v4, unpublished**.

### Per-biome unmask depth

All ten: `keep_deg = 10.0` (`keep_v = 0.0556`). Official approved `keep_land_frac` is listed only for comparison — the test files do not follow it.

| biome         | unmask ° | official keep | object handling in the band                          |
|---------------|----------|---------------|------------------------------------------------------|
| desert        | 10.0     | 0.055         | local fog mask (camp / hut / thatch / fence at seam) |
| savanna       | 10.0     | 0.055         | none (no man-made in the band)                       |
| mediterranean | 10.0     | 0.055         | local fog mask (inhabited hills, field house, right hillside) |
| jungle        | 10.0     | 0.03          | none                                                 |
| forest        | 10.0     | ~0.031        | local fog mask (farm ridge + dirt path / track)      |
| grass         | 10.0     | 0.05          | tight Telea on two posts (a fog stamp looked worse)  |
| steppe        | 10.0     | 0.065         | tight Telea on two figures; see sky exception        |
| mountain      | 10.0     | 0.055         | none                                                 |
| ice           | 10.0     | 0.025         | none                                                 |
| tundra        | 10.0     | 0.04          | none                                                 |

### Local-mask recipe

Sky above `horizon_v` is never written (restored from approved). Coverage is limited to the object itself plus a short feather — not a fog column down the rest of the photo land (that wiped the forest hillside and wrecked the join). If the object ends within 32 px of official fog-start, the bank is extended to meet it. Official fog is gradual: rows between `y_fog` and the first uniform fog row `yf` still contain the objects, so fill colour is a very wide Gaussian (`sigmaX=180`) of the uniform `yf` row, used until `yf`, then approved pixels below. Alpha: ramp in, hold, ramp out, times a wrap-safe 2-D distance-transform x-weight (`x_feather=56`, `y_feather=22`, `y_feather_out=28`).

Masked regions (TEST files only):

- **mediterranean** — one raised bank over the inhabited left/mid hills (`x≈40–1300`, from `yh+2` through official keep into uniform fog), plus the field house and farmyard to its right, a mid pole/track, and the right-hand hillside (`x≈3680–4095`). The 534 sky-pixel edits from the earlier clone pass were **reverted**; source-array sky is byte-identical to approved (`sky_diff_px=0`).
- **forest** — one continuous ridge band over the farmstead (`x≈3010–3548`; a connecting rect so ellipse gaps do not print as dark vertical seams) and the dirt path / brown track (`x≈3810–4050`, `y≈998–1130`). Valley ponds were left (water, not tents). The green slope below the farm is kept.
- **desert** — rondavel and wrap, smaller dark hut, thatch under the tree, fence posts through the keep sliver to uniform fog (`yf=1050`).
- **grass** — two posts, tight Telea, bush intact.
- **steppe** — two figures sit *just above* `horizon_v` (hv y=1056, figures y=1026–1056). A below-horizon mask cannot hide them, so the tight Telea and the **832 sky-pixel exception** stay. Revert was not possible without a sky edit.

### Size (lossy q=90)

| file            | 4k     | 2k    |
|-----------------|--------|-------|
| desert.webp     | 98 KB  | 44 KB |
| savanna.webp    | 101 KB | 41 KB |
| mediterranean.webp | 96 KB | 38 KB |
| jungle.webp     | 215 KB | 77 KB |
| forest.webp     | 133 KB | 54 KB |
| grass.webp      | 242 KB | 107 KB |
| steppe.webp     | 109 KB | 45 KB |
| mountain.webp   | 82 KB  | 33 KB |
| ice.webp        | 198 KB | 72 KB |
| tundra.webp     | 76 KB  | 31 KB |

All ≤600 KB and well under 2 MB. Visually checked in the engine frames; no obvious encode damage at q=90.

### 4-yaw verification

All 80 frames (10 biomes × 2 cameras × 4 yaws) were rendered at 1280×720 and viewed at full size, plus composed horizon strips in `review/ground/yaw4_strips/`. No building, house, road or fence is visible in the unmasked band at any of those views.

A few pale structures sit on mediterranean hills that already rise *above* `horizon_v` in the approved photo (the official horizon cuts through mid-slope). Those pixels are in the approved sky region, were already in the v2 raised frames, and were left untouched so the sky stays source-array identical.

## What changed in the first v4 pass (and what did not)

The v3 idea is unchanged and was confirmed: test backdrops in `ground/backdrop_test/` keep the photo’s real land ~10° below `horizon_v`, and the ground disc alpha-fades into that land (`transparent` + `depthWrite`, fade r = 760–1480, rim 4). Alpha-hash stays rejected. Play area, keep, walls, lighting, and locked v2 textures/tints are unchanged.

The first v4 was three polish items (shader and foot-lerp table are locked; this later pass did not retune them):

1. **Object cleanup in the TEST backdrops only.** First pass used tight Telea for thin posts/figures and wrap-aware clone-stamp for larger structures. Large Telea and large clone ovals were tried and rejected — they smeared or left visible stamps. Leftover hillside houses and the forest farmstead after that pass are what the local masks above now hide.
2. **Pale misty join band on mountain and savanna.** Diagnosed as the optical mix of a dark fading disc over the photo’s own pale haze, with scene fog a secondary wash. Fix: cancel scene fog in the fade ring only (play area keeps v2 fog), and lerp the disc toward a sampled photo foot colour in that ring.
3. **Tone step on ice, jungle, grass.** The same fade-ring foot lerp, with per-biome strength. No further tone tuning after the first v4 read.

## Pale-band diagnosis and fade-ring tone match (locked)

Mountain photo-foot luma ≈ 127 vs disc ≈ 53 (ΔL ≈ +74). Savanna ≈ 113 vs 72 (ΔL ≈ +41). The mid-fade composite of a dark disc over pale photo haze reads as a pale strip. The photo itself also has atmospheric haze at the hill / valley foot; that stays, and should.

Shader (`viewer.html`, cache key `ground-v4b-`):

- After fog: `gl_FragColor = mix(preFog, fogged, fadeA)` so the fade ring is unfogged and the play area keeps v2 fog.
- Over the fade ring only: luma-lift + 45% chroma toward a per-biome foot colour sampled from the v3 raised frames’ photo-land strip (y≈136–144, mid 70% width). Shipped weight: `smoothstep(0, 0.40, ring) * footLerp`.

Foot colours and strengths (fade ring only, **locked**):

| biome          | foot    | lerp | intent                                      |
|----------------|---------|------|---------------------------------------------|
| desert         | #746b62 | 0.30 | mild                                        |
| savanna        | #886f56 | 0.80 | kill mix strip                              |
| mediterranean  | #5e5945 | 0.40 | mild                                        |
| jungle         | #4a5535 | 0.80 | lift toward sunlit canopy foot              |
| forest         | #2a2e26 | 0.12 | already close                               |
| grass          | #708160 | 0.42 | slight, toward browner hill foot            |
| steppe         | #81776c | 0.35 | khaki ridge (not the sky/haze above it)     |
| mountain       | #817f7b | 0.85 | kill mix strip                              |
| ice            | #95a4a2 | 0.85 | brighter, toward the glacier                |
| tundra         | #686869 | 0.20 | already close                               |

Mid-fade luma residuals on the raised frames (photo foot vs mid-fade row, v3 → first v4): savanna +14.3 → +1.0; mountain +40.4 → +12.8; ice +28.7 → +9.1; jungle +35.8 → +17.6; grass +50.6 → +39.2 (intentionally slight).

## Phone / sorting notes (unchanged from v3)

The entire disc is in the transparent pass. Keep and walls are opaque and drawn first, so they write depth; the ground still depth-tests and stays behind them. `depthWrite` stays on so the opaque play area occludes units. Backdrop sphere: `MeshBasic`, `toneMapped=false`, `fog=false`, `BackSide`, `depthWrite=false`, `renderOrder=-100`. Numerics are inlined as GLSL consts (custom uniforms do not bind reliably in r186).

## Review (honest, not a self-pass)

I reviewed the 20 default-yaw battle/raised frames at 1280×720, a 1:1 join crop, and all 80 four-yaw frames at full size (both cameras). I do not pass this for Michael or the COO.

- **desert** — camp / hut / thatch / fence are gone under a local fog bank, confirmed at yaw +270 (the seam yaw) on both cameras. The bank is a readable gray patch with a slightly scalloped top (it follows the hut silhouette and nips a little of the tree). Soft atmospheric haze, no hard keep-line. Sand still a touch khaki versus the sunlit photo floor (locked v2 colour).
- **savanna** — clean in all 4 yaws, both cameras. Pale mix strip remains largely gone; the photo’s own valley haze remains, as it should.
- **mediterranean** — hillside village, field house, farmyard, and right-hand houses in the unmasked band are gone under raised official-style fog banks, confirmed at all 4 yaws on both cameras. The bank is a readable horizontal gray bar on the inhabited left/mid hillsides, most obvious on the raised camera at yaw +0 and +90. That is a look cost of hiding houses that sit in the official keep sliver, not a tone issue. A few pale structures sit on hills that already rise above `horizon_v` in the approved photo and were left so the sky stays byte-identical. Source-array sky is identical to approved.
- **jungle** — clean of man-made objects in all 4 yaws. The remaining mid-fade brightness step is still readable (leaf-litter vs sunlit canopy); not retuned.
- **forest** — farmstead and dirt path / track are gone under local fog banks, confirmed at raised/battle yaw +180 (farm) and +270 (path). The green slope below the farm is kept. The banks read as mid-field gray haze patches at those yaws. Default yaw is still a clean green-hill join. Valley ponds left (water).
- **grass** — posts still gone. A horizontal value step is still clearly visible on the raised camera (the locked lush-green disc and the hill foot differ by ~90 luma). Not retuned.
- **steppe** — figures still gone. Soft fade into the khaki plateau. The 832 approved-sky pixels on the ridge crest stay as the documented exception; they cannot be reverted without showing the figures again.
- **mountain** — clean of man-made objects in all 4 yaws. The pale mix strip stays reduced; the photo’s own dusty valley haze is still there and should be. Not retuned.
- **ice** — clean of man-made objects. Soft remaining dark-to-light step, milder than v3, still just readable on the raised camera. Not retuned.
- **tundra** — clean of man-made objects in all 4 yaws. Soft fade into the snowy forested ridge.

Remaining honest gaps: the grass tone step, the jungle brightness step, and the mountain photo haze — plus the visible raised-fog banks on mediterranean, forest, and desert at the inhabited yaws. Textures stay locked. Nothing is published. I do not self-pass.
