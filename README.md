# PHerc1451: automatically grown surfaces, release v3

Licence: CC BY-NC 4.0 (see LICENSE). Candidate surfaces, not finished segments; no ink result.

57 automatically grown surfaces for PHerc1451 (scan 20260319101107, 2.399 um), in tifxyz, level-0 voxels: the 40 v2 surfaces and 17 new ones; 38 of the 57 pass the fibre check, the self-check and the winding cross-check where they overlap; the 19 that fail the fibre check are in their own folder. All 57 open with villa's own loader (vc_tifxyz2obj, villa 405f3f32d; evidence/vc_load_check_v3.tsv); we did not open the VC3D GUI. Surface geometry only: no ink result, no claim of visible text.

**What changed since v2:** 17 new surfaces from two seeding batches on parts of the scroll no earlier surface reached: 40 seeds grew 18 surfaces, 13 of which pass the fibre check, and a second batch added 4 more that pass. The self-check for sheet jumps was applied to each before counting, and each was then moved to the CT density centre with the same kind of move as the v2 surfaces (99.6-100 % of vertices kept, 0.49 % of area lost, 1.08 cm²). The v2 surfaces are unchanged. Pictures of the six largest surfaces (flattened CT and CT cross-sections with the surface drawn, an offset control, and one known failure before and after trimming) are in `gallery/`.

**How much passes our checks:** 38 surfaces pass the fibre check and the self-check, covering about 220 cm² with overlaps counted once (217-228 cm² depending on the de-duplication tolerance): the 21 v2 surfaces (about 130 cm²) plus 17 new ones (about 90 cm² of new area, 88-94). All 57 shipped surfaces, including the 19 that fail the fibre check, cover about 274 cm² (271-282). The total was reproduced in a second run (the same de-duplication code, with independently rebuilt trim masks). One new surface is larger than 20 cm² (21.1 cm², by the same area method as the totals). This is an upper bound, not a verified one-sheet area: two surfaces jumping at the same spot would not be caught.

**How the new surfaces were found (method; one run each):** 12,000 new random points on the organisers' m7 sheet prediction were scored with the same site score as before; none of the 288 high-scoring points lay far from the 160 sites already grown, so we looked for sheet no grown surface reaches instead. A point counts as untouched if no grown surface lies within 4 voxels (this test finds the 160 known seeds as grown 91 % of the time); about 960 cm² of inner and middle sheet was untouched (rough, within a factor of 2). Seeds were untouched points with a site score of at least 0.5, at least 300 voxels apart, best first. Growth and certification used the same tools as v1. The first 40 seeds grew 18 surfaces, 13 of which pass the fibre check (none of 18 deliberately shifted copies passed); the next 64 grew 13, of which 4 pass (none of 13 shifted copies passed). All 17 then passed the self-check and the winding cross-check (0.52 cm² trimmed in total across the 17). Lower-scoring seeds mostly fail the fibre check, so a planned third chunk was stopped. Compute: about 17 VM-hours of Azure credit (about $18, list price). "New" in our seeding tables means more than 4 voxels from every earlier grown patch; the area above counts overlaps once against the released surfaces instead.

**Which sheet:** winding identity is not independently verified for any surface, v2 or new; what we have is agreement between growths of the same method. For the v2 surfaces: the winding cross-check between overlapping units (42 of 43 planted skips caught; an independent re-implementation 49 of 51; per-vertex winding numbers for the seven central units are relative turn numbers, not verified identity) and the seven central units agreeing over 33.2 of 33.7 cm². The new surfaces overlap released surfaces on only about 12 cm² (about 8 % of their area), where the cross-check found no conflict; the rest is not tested by it. Whether neighbouring new surfaces lie on one winding is UNRESOLVED: we make no claim that any two new surfaces follow one winding, and no merged surface is shipped.

**Layer centring:** the v2 surfaces were moved to the CT density centre (21 of 21 read as centred, within one voxel). As grown, 11 of the 13 first-batch new surfaces sat 2-4 level-2 voxels (about 20-40 um) off the layer centre, with mixed signs. After the move, all 17 new surfaces sit on the layer peak, level with the v2 surfaces (pooled read 0.546 against 0.435 and 0.439 for copies shifted 5 voxels either way). This shows each surface is centred on a CT layer, not which sheet it is.

**Limits:** v2 limits stand (the fibre check cannot tell on-sheet from in-gap; t3g_14 withdrawn in v2; t3y_09 runs about 1-1.5 mm through a gap). The self-check catches a surface coming back onto a layer it already covered; it cannot separate two turns closer than 24 um (they look like self-overlap), and a step onto the next layer is invisible to it. The winding cross-check sees only places where two surfaces overlap. So a smooth drift onto an adjacent layer that no other surface covers is caught by neither check (shown on a planted test). A third check, that winding offsets close around loops of overlapping surfaces, replicates in two independent implementations (0 non-zero loop residuals) but missed a planted physical swap (half a surface moved out one sheet) on 15 of 19 units, because the larger unmoved part outvotes it; loop closure is therefore NOT evidence of winding identity. Treat all surfaces as candidates to check, not finished segments.

**What villa #2605 is:** a docs pull request that lists this dataset on the Vesuvius Challenge community-projects page.

**Licence:** CC BY-NC 4.0 (derived from a CC BY-NC 4.0 scan); scripts MIT. Please cite the dataset as https://scrollprize.org/data asks.

## Quick start (about 5 minutes)

**1. Get the surfaces.** Download `pherc1451_surfaces_v3.tar.gz` from the v3 release, then:
```
tar -xzf pherc1451_surfaces_v3.tar.gz && cd pherc1451_surfaces_v3 && shasum -a 256 -c SHA256SUMS
```
**2. Look at one in Python** (needs `numpy` and `tifffile`):
```python
import json, numpy as np, tifffile
d = "surfaces/gated/w1451_11-on-20260319101107-2.399um.tifxyz"
x, y, z = (tifffile.imread(f"{d}/{c}.tif") for c in "xyz")   # level-0 voxel coordinates, -1 = no vertex
valid = x != -1
print(x.shape, valid.sum(), json.load(open(f"{d}/meta.json"))["scale"])
```
**3. Open in VC3D.** The scan is the public volume
`https://vesuvius-challenge-open-data.s3.us-east-1.amazonaws.com/PHerc1451/volumes/20260319101107-2.399um-0.2m-78keV-masked.zarr`.
Make (or reuse) a volpkg that points at that volume, copy the `.tifxyz` folders into its `paths/`, and open the volpkg in VC3D
(the loader VC3D uses was tested on all 57 with villa 405f3f32d; we did not open the VC3D GUI). Or render one directly: `vc_render_tifxyz` / `vc_tifxyz2obj` on any folder.
**4. Coordinates.** x/y/z are level-0 voxels (2.399 um); one grid step is 80 level-0 voxels (`scale` 0.0125). `INDEX.csv` has one row per surface.

## Pictures
`gallery/GALLERY.md`: for the six largest surfaces, a flattened CT view and three CT cross-sections each with the surface drawn, an offset control, and t3y_09 before and after trimming (a known failure, shown on purpose). CT intensity only; no ink result or text claim.

## Files
- `surfaces/gated/`: the 38 surfaces that pass the checks; `surfaces/fail_fibre_check/`: the 19 that fail the fibre check. tifxyz (x, y, z float32, level-0 voxels; masked vertices are -1). `INDEX.csv` lists each with its batch, fibre-check result, gated flag and area.
- `evidence/`: relative winding numbers for the seven central units (`winding/`), turn counts (`turns_umb.json`), winding cross-check results (`i13_umb.json`, `i13c.json`), the z cross-section figure (`zslice_core.png`, geometry only, no CT) and the area total (`moved2.json`).
- `LICENSE` (CC BY-NC 4.0 legal code), `LICENSE.md` (summary), `CITATION.cff`, `SHA256SUMS` (every file).

GitHub's licence badge may not recognise CC BY-NC; the licence is CC BY-NC 4.0 (LICENSE).
