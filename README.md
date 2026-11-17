# PHerc1451: 41 automatically grown surfaces, 192.36 cm², placed at the centre of the papyrus layer

Release v1. Surfaces for PHerc1451, scan `20260319101107` (2.399 um), in the Vesuvius Challenge tifxyz format. All 41 open in villa's
own loader. This is surface geometry only: **no ink result and no claim that any text is visible.**

## Download
The surfaces are in the release asset **`pherc1451_surfaces_v1.tar.gz`** (12.4 MB): see [Releases](../../releases/latest).
sha256 `d1d8d2a8ecbc680cbf99f3c7f56dc115092f47fd8fa402d15730ac4ad21a2ee7`. Unpack it and run `shasum -a 256 -c SHA256SUMS` inside the folder.
This repository page shows the README, the index table and the two pictures; `surfaces/` and `evidence/` named below are inside the archive.


**What this is.** 41 surface patches on PHerc1451, scan `20260319101107` (2.399 um), a scan with no published segments. Each was grown
automatically with `vc_grow_seg_from_seed` on the organisers' m7 surface prediction, kept only where three independently grown patches
agree within 2 level-2 voxels, then moved vertex by vertex (CT only) to the density centre of the papyrus layer it traces.

**Use it.** Each folder in `surfaces/` is a tifxyz named the way the PHerc0139 meshes are, `<unit>-on-20260319101107-2.399um.tifxyz`,
with x/y/z in **level-0 voxels** of `PHerc1451/volumes/20260319101107-2.399um-0.2m-78keV-masked.zarr`. Copy the folders into a volpkg's
`paths/` and open them in VC3D, or pass one to `vc_render_tifxyz` / `vc_tifxyz2obj`. `INDEX.csv` has one row per unit; `SHA256SUMS`
covers every file.

## Measurements (MEASURED = our run, file named; CALC = arithmetic on measured numbers; one run each unless stated)
| What | Number | Label, source |
|---|---|---|
| Certified area in these 41 surfaces, overlaps counted once | 192.36 cm² | MEASURED on the grown surfaces (INDEX.csv, column certified_cm2_dedup_grown, sums to this). 13 further small sites (7.55 cm²) passed the same check but have no centred surface and are not in this release |
| Area inside the 41 files (overlaps between units counted twice) | 255.78 cm², 750,688 vertices | MEASURED, evidence/stage1_package.jsonl |
| Units clearing a cross-validated fibre check on the moved surface | 22 of 41 = 138.35 cm² | MEASURED (organisers' fibre prediction, never read by growth or move; bar 1.373; held-out organiser meshes 6/6; shifted control 0/10, 0/11) |
| Moved surface within 3 voxels of an m7 ridge | 146.08 cm² | CALC from MEASURED shares (INDEX.csv, m7_ridge_share_moved) |
| Move raises fibre contrast at the surface | 30 of 32 units; on units new to the move 11/12 and 9/11 | MEASURED (three rounds, rule written before each); moving to the gap centre instead lowers it 20/20, 12/12, 11/11 |
| Median distance from surface to fibre peak, before -> after the move | 3 -> 1 level-2 voxels (29 -> 10 um) | MEASURED |
| Opens with villa's own loader (`vc_tifxyz2obj`, villa 405f3f32d) | 41 of 41 | MEASURED on these exact files, evidence/vc_load_check_package.tsv |
| VC3D loader rules; vertices outside the scan | 41 of 41 pass; 0 outside | MEASURED, evidence/stage1_package.jsonl |
| Written with villa's `vc_reader.tifxyz` (f29334cd8); read back v our source | identical masks; max difference 0.0005 level-2 voxel | MEASURED, evidence/build_roundtrip.jsonl |

## Coordinates: level 0 = 4 x level 2 + 1.5 (measured, not assumed)
The surfaces were made on the level-2 grid. A plain x 4 would put them 1.5 level-0 voxels (3.6 um) off on each axis, because this scan's
level-2 voxels are 4x4x4 block means. Paired read, level-0 CT at the package position v level-2 CT at the source position
(evidence/placement_run*.json, tools in evidence/):
| Position tested | Correlation, run 1 (14 units, 504 points) | Correlation, run 2 (14 other units) |
|---|---|---|
| shipped: 4q + 1.5 | 0.990 | 0.986 |
| half a voxel either side | - | 0.978 / 0.978 |
| plain 4q | 0.930 | 0.925 |
| 4q + 3 | - | 0.920 |
| sabotage: 8 voxels off | 0.468 / 0.290 (run 1 measured them from plain 4q) | 0.392 / 0.396 |
Shipped was the closest on 14 of 14 units in run 1 and in 13 of 14 against each half-voxel arm in run 2. To get our level-2 working frame back: (p - 1.5) / 4.

## Limits (please read)
1. **Which sheet is not shown.** The move fixes where in the layer a surface sits, not which layer. Our sheet-jump detector failed its
   bench and a blinded look at earlier crops scored 1 of 6. Some units cross between strands in tangled regions (e.g. t3h_06).
2. The fibre prediction is itself CT-derived, so some agreement is expected; the sabotage arm losing every round is the stronger evidence.
3. The move targets the CT density centre, about 1 level-2 voxel inside the sheet from the recto face the organisers' meshes follow.
   Not compared with `snap_labels` (villa PR #2368).
4. Certification (192.36 cm²) was measured on the grown surfaces; the moved area is 0.998-1.008 x grown per unit (CALC).
5. Units overlap each other (255.78 cm² in files v 192.36 counted once). Not checked against the organisers' 17 public PHerc1451
   patches, which are in another scan's frame. 8,969 valid vertices (1.2 %) sit in no complete quad and carry no area.
6. PHerc1451 only. No ink claim.

## Pictures
`CONTACT_SHEET_41_ALL.jpg` (all 41 units rendered at level 0); `t3h_07_row0.5.png` (cross-section: red = grown surface in the air gap,
green = moved surface inside the sheet).

## Related tool PR
https://github.com/ScrollPrize/villa/pull/2594 (draft): post-fit CT-ridge nudge for spiral fits, the same idea of placing a surface
by the CT applied to fitted checkpoints.

## Licence and credit
- **Surfaces, INDEX.csv and the evidence files: CC BY 4.0.** Credit: "PHerc1451 surfaces v1, EconLearn, 2026".
- **Pictures** (`CONTACT_SHEET_41_ALL.jpg`, `t3h_07_row0.5.png`) are rendered from the scan, so they carry the scan's licence, **CC BY-NC 4.0**.
- **Scripts in `evidence/`: MIT.**
- The scan and the surface and fibre predictions these surfaces were grown and checked on are Vesuvius Challenge / EduceLab data
  (CC BY-NC 4.0, https://scrollprize.org/data). If you use the surfaces, cite the Vesuvius Challenge dataset for PHerc1451 as well.
- Tools used: `vc_grow_seg_from_seed`, `vc_render_tifxyz`, `vc_tifxyz2obj` and `vc_reader` from https://github.com/ScrollPrize/villa.

## Feedback
If you open, render or run ink detection on these surfaces, an issue on this repository saying what worked and what did not is very welcome.
