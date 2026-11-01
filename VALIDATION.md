# V3 notebook validation

Validated against the public V2 repository snapshot recorded in README_START_HERE.md.

Executed checks:

- Notebook 01 executed using the repository's real preparation data and frozen hashes.
- Exact exclusion of 747 fallback events: 193 training, 170 validation, 148 calibration,
  111 policy and 125 test. Retained 11,323 events, with unchanged event identities, labels,
  camera assignments and relative order. No top-up or resampling.
- Frozen preparation rerun produced identical inputs.
- Full cache-copy and readiness workflow executed using 11,323 synthetic 224x224 RGB PNGs
  and the actual event manifests. Synthetic images were test fixtures, not wildlife observations.
- Repeated cache verification and complete Notebook 03 dataset preflight passed on that fixture.
- Deliberately corrupting a PNG blocked training.
- Reintroducing a fallback event or the old no-detection rule blocked training.
- Real V2 preparation file hashes remained unchanged throughout verification.
- Notebook 04 temperature fitting checked against an independent SciPy minimizer;
  calibration NLL did not increase, and accuracy/macro-F1 remained unchanged.
- Binomial-bound zero-error boundary checked (193 versus 194 accepted events), along with
  undefined zero-acceptance risk and safe no-qualifying-threshold behavior.
- All code cells in all five final notebooks compile. All execution outputs are cleared;
  no V2 result tables or execution traces are presented as V3 results.

Runtime limits:

The authoring environment has no PyTorch/torchvision and no original wildlife image cache.
CNN training, checkpoint resume/loading and full model inference were not executed here.
Notebooks 03–05 retain the existing V2 numerical recipes, with V3 paths/provenance gates,
detected-only population checks and revised reporting. End-to-end V3 model results must
be produced in the user's existing ML environment. No trained V3 models are included.

Notebook 02 intentionally requires the complete local V2 cache, which the GitHub repository
does not include. Missing or changed image files fail explicitly rather than being silently
dropped, downloaded or converted to full-frame fallbacks. Optional training-only visual
review is provided; no semantic label verification is claimed automatically.

Documentation restoration: restored detailed V2 training/evaluation/audit explanations, adapting only affected passages for V3, and added complete explanations for the revised preparation/cache steps. Executable code-cell SHA-256 lists were compared before and after the edit and are identical in all five notebooks.
