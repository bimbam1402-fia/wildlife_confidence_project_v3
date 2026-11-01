# Wildlife confidence project V3 — detected crops only

Source: https://github.com/bimbam1402-fia/wildlife_confidence_project_v2
Source commit: c652ca64b1bed6ab2528e1a43758b279778f2a00

## Start now

1. Extract this folder beside your complete LOCAL V2 project. Use your existing `ml` Conda environment.
2. Open `01_Wildlife_Detected_Only_Data_Preparation_v3.ipynb` and Run All.
   Set `SOURCE_PROJECT_ROOT` if discovery fails: it is the V2 folder containing `data/prepared` and
   `data/images`, often the inner folder in the GitHub layout. V3 must be outside that V2 data root.
3. Run `02_Wildlife_Crop_Cache_and_Quality_Checks_v3.ipynb`. It copies/verifies existing crop PNGs
   without downloading images. Check the training-only contact sheet if time permits.
4. Run `03_Wildlife_Training_and_Development_Logits_v3.ipynb` in the same folder/environment.
   `RUN_LIMIT=None` attempts all six runs; CPU is the default. Leave it running.
5. After all six runs finish, run Notebook 04 V3, then Notebook 05 V3.

The V3 output root defaults to the folder containing these notebooks when it is named
`wildlife_confidence_project_v3`. Otherwise it creates that subfolder in the working directory.
All later notebooks find that root automatically or accept explicit `PROJECT_ROOT`.
Keep the same working directory across notebooks, and restart the kernel between notebooks.

## Expected dataset from this repository snapshot

| Partition | V2 | Excluded fallbacks | V3 animal crops |
|---|---:|---:|---:|
| Train | 4,570 | 193 | 4,377 |
| Validation | 1,500 | 170 | 1,330 |
| Calibration | 1,500 | 148 | 1,352 |
| Policy | 1,500 | 111 | 1,389 |
| Test | 3,000 | 125 | 2,875 |
| Total | 12,070 | 747 | 11,323 |

Every retained ID, frame, class label, camera and partition comes from V2. No replacement or
top-up is performed. The detector threshold (0.20), highest-animal-box rule, 5% margin and 224px
preprocessing remain unchanged. Only `animal_crop` is allowed. Technical detector errors are not
silently treated as exclusions. All outputs/checkpoints are separate; no V2 trained weights are reused.

## Scope and comparison

This is classification conditional on successful detection, not empty-scene detection or unfiltered
camera-stream evaluation. No-detection does not prove emptiness. Crops can still have semantic errors.
Original camera allocation/provenance files retain their V2 identifiers because they are inherited
unchanged; V3 input, selection, training, evaluation and audit protocols identify the revised study.
Original camera caps applied before exclusions; retained shares can exceed 20% in development.
Prior V1/V2 test inspection informed V3, so its test cameras are not a new independent confirmation.
Calibration/policy/test are still camera-disjoint from training within V3. Never tune against V3 test results.

## Resuming and disk use

Preparation reuses verified copies. Training resumes epoch-boundary V3 checkpoints and verifies/skips
completed runs. Run All again after interruption; do not delete protocol files to bypass a mismatch.
Cache copies need extra disk space. GitHub does not contain the image cache; use your complete local V2
project as source. Missing/corrupt caches produce an explicit failure report, with no silent exclusions.

## Validation performed by the author

See `VALIDATION.md` for executed checks and runtime limitations. This package does not contain
trained V3 weights or claim V3 results before you run it.
