# Calibration and uncertainty-aware policy evaluation — v3 actual results
Generated 2026-11-01T12:23:02.317002+00:00. Evaluation hash: 9bb2a0020a584ff5aa70c847d9f4397a4bd931ae8685412fd5c0ac39e97ee2b2. Frozen decision hash: a4cafae5ee4bd5f8001ee1229acf4be3a16ecf359cbb56672643f35915ab79a0.
Experiment: five_categories_detected_only_v3. Training ID: training_v3. Evaluated all 6 planned checkpoints.

## Calibration (separate camera partition)
Each corrected Notebook 03 v3 selected head-or-fine-tuning checkpoint is kept unchanged. One positive scalar temperature per run fitted by minimizing **image-weighted calibration NLL** within [0.01, 100.0]. Calibration fitting does not alter the CNN or argmax labels. The calibrated probability quality is evaluated independently on the held-out test split.

## Binomial-bound threshold selection (secondary analysis; policy cameras only)
For each raw and scaled model/seed output, compute confidence as the maximum softmax probability. Accept if confidence >= threshold and abstain on the rest; no manual image review is performed in this experiment. We fixed 85 cutoffs in the interval [0.2, 1.0] before reading policy outcomes, including a finer high-confidence tail. Candidates are not extracted from policy observations. Ties are accepted together.

For each candidate, observe n accepted policy images and e wrong predictions, and compute a one-sided exact Clopper–Pearson 95%-familywise *binomial* upper bound on its unknown accepted error. Bonferroni uses alpha = 0.05/(12 policies x 85 cutoffs) = 4.90196078e-05 per test. Binomial exactness assumes independent observations at a fixed cutoff. The multiple-testing adjustment allows threshold selection among all fixed cutoffs under that independence model.

For camera diagnostics only, we also ran 10000 seeded whole-policy-camera resamples over 13 distinct policy cameras, shared across all policies. We computed an approximate standardized-bootstrap critical value 3.70845 from the largest standardized positive bootstrap deviation across eligible cutoffs and policies, then formed per-cutoff approximate simultaneous camera-bootstrap upper estimates only where nondegenerate evidence exists. Zero observed errors, zero/near-zero variation and nonfinite estimates imply an unavailable camera diagnostic, not zero camera uncertainty and not rejection of the threshold. All images in a sampled camera stay together, and error/coverage remain image-weighted within the sampled mixture. Bootstrap replicates with no accepted images retain the original 100%-risk assignment in this diagnostic. Neither camera-estimate availability nor its value is used in selection.

The selection criterion uses only **binomial_bonferroni_ucb_95 <= 5%**, with >= 100 accepted images and >= 5 contributing cameras. Zero-error candidates are allowed when this bound passes. For schema compatibility, screening_upper is an alias of the binomial bound. Camera-bootstrap estimates remain separate diagnostics and never veto a candidate. Among passing candidates, choose the greatest number of accepted images; tie-break by the higher cutoff. No passing cutoff means no automatic predictions (undefined accepted error), not zero-error success. 12/12 model/seed/variant policies selected a cutoff on the v3 policy data. Each candidate's observed risk and both upper bounds are available in its policy_candidates CSV. Raw and scaled policies were selected separately. All selections and temperatures were frozen before inference on test images.

**Interpretation of uncertainty:** The Clopper–Pearson bound is exact for independent accepted images and fixed cutoffs (with Bonferroni protecting the fixed-grid search), but the events are clustered within cameras. The camera-bootstrap max-t upper bound acknowledges camera structure, but with a modest number of cameras it is *approximate*, not an exact finite-sample interval. Selection by the binomial bound is assumption-dependent, **not a mathematical guarantee of <=5% errors at a newly deployed camera**. A missing camera diagnostic remains explicitly missing even when a threshold is selected; a finite diagnostic is not proof of valid camera-aware coverage. All risk estimates describe the available selected event/camera distributions; shifts in camera mix, animal class prevalence and unseen locations can defeat transfer. A policy with no threshold passing this screening rule may indicate insufficient evidence rather than a poor classifier.

## Temperatures (fitted development split)
```text
             model  seed  temperature  calibration_nll_raw  calibration_nll_scaled  at_temperature_boundary
mobilenet_v3_large    42       1.1408               0.4517                  0.4485                    False
   efficientnet_b0    42       0.7508               0.3482                  0.3347                    False
mobilenet_v3_large   123       1.4710               0.3859                  0.3587                    False
   efficientnet_b0   123       0.8792               0.4173                  0.4141                    False
mobilenet_v3_large  2026       1.5461               0.5170                  0.4753                    False
   efficientnet_b0  2026       1.2257               0.3217                  0.3152                    False
```

## Threshold decisions (policy split, with upper confidence bounds)
```text
             model  seed variant  temperature   status                               reason  threshold  accepted  errors  accepted_cameras  coverage  accepted_error  binomial_bonferroni_ucb_95  camera_uncertainty_available      camera_uncertainty_reason  camera_bootstrap_degenerate  camera_bootstrap_sd  camera_bootstrap_simultaneous_upper_approx  screening_upper
mobilenet_v3_large    42     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9900       574      10                12    0.4132          0.0174                      0.0494                          True approximate_estimate_available                        False               0.0085                                      0.0489           0.0494
mobilenet_v3_large    42  scaled       1.1408 selected passes_binomial_bonferroni_screening     0.9900       505       8                12    0.3636          0.0158                      0.0498                          True approximate_estimate_available                        False               0.0080                                      0.0453           0.0498
   efficientnet_b0    42     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9400       783      15                13    0.5637          0.0192                      0.0459                          True approximate_estimate_available                        False               0.0082                                      0.0497           0.0459
   efficientnet_b0    42  scaled       0.7508 selected passes_binomial_bonferroni_screening     0.9800       777      13                13    0.5594          0.0167                      0.0425                          True approximate_estimate_available                        False               0.0067                                      0.0417           0.0425
mobilenet_v3_large   123     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9950       732      15                13    0.5270          0.0205                      0.0491                          True approximate_estimate_available                        False               0.0062                                      0.0435           0.0491
mobilenet_v3_large   123  scaled       1.4710 selected passes_binomial_bonferroni_screening     0.9700       737      14                13    0.5306          0.0190                      0.0468                          True approximate_estimate_available                        False               0.0063                                      0.0422           0.0468
   efficientnet_b0   123     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9200       821      18                13    0.5911          0.0219                      0.0491                          True approximate_estimate_available                        False               0.0063                                      0.0454           0.0491
   efficientnet_b0   123  scaled       0.8792 selected passes_binomial_bonferroni_screening     0.9500       813      18                13    0.5853          0.0221                      0.0496                          True approximate_estimate_available                        False               0.0065                                      0.0463           0.0496
mobilenet_v3_large  2026     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9990       548       9                11    0.3945          0.0164                      0.0489                          True approximate_estimate_available                        False               0.0187                                      0.0858           0.0489
mobilenet_v3_large  2026  scaled       1.5461 selected passes_binomial_bonferroni_screening     0.9800       592      10                11    0.4262          0.0169                      0.0479                          True approximate_estimate_available                        False               0.0190                                      0.0872           0.0479
   efficientnet_b0  2026     raw       1.0000 selected passes_binomial_bonferroni_screening     0.9700       914      20                13    0.6580          0.0219                      0.0472                          True approximate_estimate_available                        False               0.0114                                      0.0640           0.0472
   efficientnet_b0  2026  scaled       1.2257 selected passes_binomial_bonferroni_screening     0.9400       909      18                13    0.6544          0.0198                      0.0444                          True approximate_estimate_available                        False               0.0107                                      0.0594           0.0444
```

## Held-out test evaluation
2875 distinct event representatives from 28 previously excluded classifier camera locations. The five categories and camera-specific sample sizes come from the frozen v3 data selection. Accuracy, five-class macro-F1, NLL, summed multiclass Brier, 15-bin top-label ECE (10/20-bin sensitivity), coverage and accepted-error are reported without test-driven changes. If a policy accepts no images, accepted error and empirical test-target status remain undefined.

```text
             model  seed variant  temperature  images  cameras  threshold policy_status  policy_accepted_error  policy_screening_upper    nll  accuracy  macro_f1  brier    ece  accepted  accepted_errors  coverage  manual_review_fraction  accepted_error  accepted_cameras  target_met_on_test
mobilenet_v3_large    42     raw       1.0000    2875       28     0.9900      selected                 0.0174                  0.0494 0.3512    0.8654    0.7837 0.1813 0.0137      1377                5    0.4790                  0.5210          0.0036                26                True
mobilenet_v3_large    42  scaled       1.1408    2875       28     0.9900      selected                 0.0158                  0.0498 0.3515    0.8654    0.7837 0.1816 0.0155      1241                3    0.4317                  0.5683          0.0024                26                True
   efficientnet_b0    42     raw       1.0000    2875       28     0.9400      selected                 0.0192                  0.0459 0.3610    0.8675    0.7977 0.1912 0.0223      1573               12    0.5471                  0.4529          0.0076                25                True
   efficientnet_b0    42  scaled       0.7508    2875       28     0.9800      selected                 0.0167                  0.0425 0.3702    0.8675    0.7977 0.1938 0.0318      1564               10    0.5440                  0.4560          0.0064                25                True
mobilenet_v3_large   123     raw       1.0000    2875       28     0.9950      selected                 0.0205                  0.0491 0.2965    0.9023    0.8351 0.1423 0.0300      1746               12    0.6073                  0.3927          0.0069                26                True
mobilenet_v3_large   123  scaled       1.4710    2875       28     0.9700      selected                 0.0190                  0.0468 0.2749    0.9023    0.8351 0.1393 0.0138      1742               12    0.6059                  0.3941          0.0069                26                True
   efficientnet_b0   123     raw       1.0000    2875       28     0.9200      selected                 0.0219                  0.0491 0.3992    0.8480    0.7672 0.2065 0.0195      1633               16    0.5680                  0.4320          0.0098                27                True
   efficientnet_b0   123  scaled       0.8792    2875       28     0.9500      selected                 0.0221                  0.0496 0.4001    0.8480    0.7672 0.2070 0.0184      1608               16    0.5593                  0.4407          0.0100                27                True
mobilenet_v3_large  2026     raw       1.0000    2875       28     0.9990      selected                 0.0164                  0.0489 0.3060    0.8960    0.8240 0.1516 0.0198      1087                1    0.3781                  0.6219          0.0009                26                True
mobilenet_v3_large  2026  scaled       1.5461    2875       28     0.9800      selected                 0.0169                  0.0479 0.3081    0.8960    0.8240 0.1535 0.0354      1199                3    0.4170                  0.5830          0.0025                26                True
   efficientnet_b0  2026     raw       1.0000    2875       28     0.9700      selected                 0.0219                  0.0472 0.3069    0.8960    0.8186 0.1501 0.0252      1985               18    0.6904                  0.3096          0.0091                28                True
   efficientnet_b0  2026  scaled       1.2257    2875       28     0.9400      selected                 0.0198                  0.0444 0.2960    0.8960    0.8186 0.1479 0.0131      1976               19    0.6873                  0.3127          0.0096                28                True
```

## Seed summaries and category-conditioned acceptance
Accepted-error summaries are means among seeds with acceptance, with the contributing seed count,
abstaining seed count, and selected-policy seed count shown explicitly. Undefined errors remain missing.
```text
             model variant  seed_count  accepted_error_contributing_seeds  abstaining_seeds  seeds_with_selected_policy  accepted_error_mean_among_accepting_seeds  accepted_error_std_among_accepting_seeds  accuracy_mean  accuracy_std  macro_f1_mean  macro_f1_std  nll_mean  nll_std  brier_mean  brier_std  ece_mean  ece_std  coverage_mean  coverage_std
   efficientnet_b0     raw           3                                  3                 0                           3                                     0.0088                                    0.0011         0.8705        0.0241         0.7945        0.0259    0.3557   0.0464      0.1826     0.0292    0.0223   0.0029         0.6019        0.0774
   efficientnet_b0  scaled           3                                  3                 0                           3                                     0.0087                                    0.0020         0.8705        0.0241         0.7945        0.0259    0.3554   0.0536      0.1829     0.0310    0.0211   0.0097         0.5969        0.0787
mobilenet_v3_large     raw           3                                  3                 0                           3                                     0.0038                                    0.0030         0.8879        0.0197         0.8142        0.0270    0.3179   0.0292      0.1584     0.0204    0.0212   0.0082         0.4881        0.1149
mobilenet_v3_large  scaled           3                                  3                 0                           3                                     0.0039                                    0.0026         0.8879        0.0197         0.8142        0.0270    0.3115   0.0384      0.1582     0.0215    0.0215   0.0120         0.4849        0.1051
```
Accepted counts/errors, support, coverage and error rate are reported separately by true and predicted
category in test_accepted_by_category.csv. The full camera audit is deferred to Notebook 05 v3.
Manual crop verification was not performed in this experiment; automatic integrity checks do not
establish semantic crop-label agreement.

## Primary paired NLL calibration-gain comparison
Gain is raw minus scaled NLL. The architecture comparison is EfficientNet gain minus MobileNet gain,
averaged across all three paired seeds in each shared whole-test-camera resample. NLL is primary;
this comparison was fixed before test inference. Positive values favour a larger EfficientNet gain,
negative values a larger MobileNet gain. A zero-crossing interval is inconclusive, not equivalence.
```text
                                                        comparison metric  primary_metric  estimate  ci_low  ci_high  valid_replicates  total_replicates
        mobilenet_v3_large NLL gain (raw minus scaled; mean seeds)    nll            True    0.0064 -0.0026   0.0394              2000              2000
           efficientnet_b0 NLL gain (raw minus scaled; mean seeds)    nll            True    0.0003 -0.0106   0.0091              2000              2000
EfficientNet NLL gain minus MobileNet NLL gain (mean paired seeds)    nll            True   -0.0061 -0.0439   0.0097              2000              2000
```

## Held-out test uncertainty
2000 paired whole-test-camera bootstrap draws with seed 20261026 form descriptive two-sided percentile 95% intervals and architecture/temperature contrasts. Same test cameras are used in all model/seed replicates. The estimates condition on model weights, calibration temperatures and selected thresholds; they do not include retraining, split selection, or temperature/threshold refitting uncertainty. Policy-camera upper bounds are NOT the test intervals. Training seed means/std reflect a fixed camera split.

## Scope and limitations
Classifier training and selection follow the v3 one-frame-per-eligible-event, camera-stratified, development-camera-capped pipeline. The camera counts and class distributions were not copied from v1. V3 retains only official precomputed MDv5a animal crops and excludes every full-frame fallback from all five partitions. Qualifying detection does not guarantee correct species labels or semantic crop correctness. Detector training may have included Caltech images; camera holdout refers to downstream classifier training, not detector pretraining. Rare categories, differing camera environments and detector-selection effects can affect performance. Coverage is conditional on retained detected events. Original development camera caps applied before exclusions; post-exclusion shares are reported. V3 uses the inherited V2 camera assignments after prior V1/V2 test inspection, so this is not independent confirmation on fresh cameras. Training head/fine-tuning choices and checkpoint selection remain as frozen in Notebook 03 v3. Results cannot isolate architecture from its particular pretrained weights. No claim of verified future error control is made.

## Outputs and references
Tables: temperature_summary.csv, policy_summary.csv, twelve policy_candidates_*.csv, test_results.csv, test_seed_summary.csv, test_per_class.csv, test_accepted_by_category.csv, calibration_gain_comparison.csv, ece_bin_sensitivity.csv, test_camera_bootstrap_intervals.csv, paired_camera_bootstrap_contrasts.csv. Figures: test_reliability.png, test_risk_coverage.png, test_confusion_matrices.png. Audit files: evaluation_protocol.json, frozen_calibration_and_policies.json, test_exports.json. Each test-prediction creation hash is journalled immediately and checked before cache reuse; existing hashes are never re-baselined. Completed evaluations anchor the registry hash in completion.json. Input/provenance checks use explicit exceptions, not removable assertions. Model/logit files are under models/five_categories_detected_only_v3/evaluation_v3 and the training_v3 directories.

Guo et al. (2017), On Calibration of Modern Neural Networks: https://proceedings.mlr.press/v70/guo17a.html
Clopper & Pearson (1934), The Use of Confidence or Fiducial Limits Illustrated in the Case of the Binomial: https://doi.org/10.1093/biomet/26.4.404
Hoeffding/Bonferroni style multiple-threshold selection and limitations: describe the fixed grid and statistical assumptions above; no external selective-risk guarantee is asserted for this clustered setup.
PyTorch weights-only loading: https://docs.pytorch.org/docs/stable/generated/torch.load.html
MegaDetector results: https://lila.science/megadetector-results-for-camera-trap-datasets/
