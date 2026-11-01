# Corrected v3 training: training_v3
Updated: 2026-11-01T11:48:42.779482+00:00

## Prepared data and readiness
Uses the frozen detected-only Notebook 01/02 V3 outputs for five_categories_detected_only_v3. Notebook 01 excluded all full-frame fallbacks; this training notebook performs no further
splitting, sampling, cropping, exclusions or downloads. Subset SHA-256: 1c2bf0d513e9e213d8592b6c59662b4024e56d28900604abbae48732c2da7d67; input
protocol: 01d9b7d9d7cffca8e6e61bc4ed5119c8be1098a037a2610bfa88d686c381e49c; camera split: 5d65707544cdafe9ae2208ca745623e4b1713c13a5e363ba902800963510f9fb; selection: 256dbc23d26e2d35a7063eaf513bbbcc5b27daa555ebcb243871c280b5e8f213.
Selected event counts: {'train': 4377, 'validation': 1330, 'calibration': 1352, 'policy': 1389, 'test': 2875}.
Selected camera counts: {'train': 64, 'validation': 13, 'calibration': 13, 'policy': 13, 'test': 28}.
Selected category counts (metadata only, not test performance):
split       train  validation  calibration  policy  test
class_name                                              
coyote       1177         498          563     558  1292
deer         1166         386          396     382   715
fox           411          60           55      51    93
rabbit       1137         334          290     342   687
skunk         486          52           48      56    88
Manual crop verification was not performed. The optional Notebook 02 review CSV is not read
or required. Automatic checks establish file integrity and frozen-dataset consistency, not whether
the selected detector crop contains the reference category. Possible crop-label mismatches,
incorrect boxes and inadequate crops remain study limitations. The selected input population
remains five known categories, one frame per eligible event. MegaDetector uses precomputed
MDv5a predictions with documented historical Caltech training overlap; camera holdout applies
to downstream fitting, not necessarily the pretrained detector.

## Training amendment, fixed before new evaluation
This is a new training revision, derived from the V2 recipe. It uses the new detected-only
V3 dataset, all three seeds [42, 123, 2026], both architectures and their
specified ImageNet pretrained weights. Batch size 4; unweighted cross-entropy; horizontal
flips in training only; AdamW weight decay 0.0001. Two head epochs at 0.001, then up to ten
fine-tuning epochs at 0.0001, with a fresh optimizer. All classifier-module parameters train
during the head stage; feature parameters and feature statistics are frozen then.
During fine-tuning all parameters are trainable and BatchNorm running statistics are updated,
matching the original V2 training behavior. Dropout/stochastic-depth follow the original training modes.

**Limitation — BatchNorm with small batches**

Training uses a batch size of four. During fine-tuning, BatchNorm running statistics are updated from these small batches, which may introduce instability or variability in model performance. This potential effect has not been experimentally isolated.
Early stopping: three fine-tuning epochs without a strict improvement in event-weighted
validation NLL. Final selection compares best_head.pt with best_finetune.pt on that same
metric; a cross-stage tie retains the head. Within-stage ties retain the earliest epoch.
The selected weights are copied to best_selected.pt, whose stage/epoch, candidate hashes
and source checkpoint hash are recorded. All development logits come from that exact file.
Equal-camera validation NLL is recorded each epoch as an additional diagnostic;
they do not change selection after seeing results. No calibration/policy/test performance
selects models or training settings. Test input integrity checks are not test evaluation.
The two architectures have different heads and pretraining recipes; this is not a causal
architecture-only comparison. Training drop_last=True omits a changing incomplete last batch;
validation and exports use every row without augmentation. Same prepared inputs/normalization
are used in training and downstream inference. One split and three training seeds do not
measure uncertainty across refitted datasets or new deployment cameras.

## Actual execution and provenance
Completed 6/6 runs. Pending/failed runs remain visible in
training_run_summary.csv. Backend: cpu; versions: {'python': '3.11.16', 'torch': '2.14.0+cu130', 'torchvision': '0.29.0+cu130', 'numpy': '2.4.6', 'pandas': '3.0.5', 'pillow': '12.2.0'}.
Protocol SHA-256: 89d3b763f7b3383edf741ac43a89cc0dafc475fbd50c82f041f2f7c3bf15ab74. Resume is at completed epoch boundaries with optimizer and
RNG state; older training_v2 runs are not resumed by training_v3. Completed runs
require all three verified development exports, matching run identity and candidate/selected
checkpoint hashes. Automatic input integrity and dataset-consistency checks do not overwrite the prepared data.
Files per run: history.csv, best_head.pt, best_finetune.pt, best_selected.pt, last_resume.pt,
completed.json, validation_logits.npz, calibration_logits.npz, policy_logits.npz. Preserve actual
weights and exports as well as their hashes. Do not claim success from the presence of a log.

## Evaluation handoff
Notebook 04 must read reports/five_categories_detected_only_v3/training_v3/training_index.json and load each
run's checkpoint_file (best_selected.pt); do not substitute best_finetune.pt or old v3 exports.
All six runs are retained. Temperature fitting and uncertainty-aware policy selection belong
to Notebook 04 and must precede final test inference. Calibration/policy camera concentration,
distribution shift and prior V1/V2 test inspection remain study limitations; this training revision
does not repair them. No final test predictions are produced here.
