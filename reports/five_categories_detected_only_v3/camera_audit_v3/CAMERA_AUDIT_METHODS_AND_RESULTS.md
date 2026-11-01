# Camera robustness audit — v3 actual results
Generated: 2026-11-01T12:24:22.014877+00:00. Experiment: five_categories_detected_only_v3; training: training_v3.
Parent evaluation hash: 9bb2a0020a584ff5aa70c847d9f4397a4bd931ae8685412fd5c0ac39e97ee2b2; frozen decisions: a4cafae5ee4bd5f8001ee1229acf4be3a16ecf359cbb56672643f35915ab79a0.

## Scope and provenance
This sensitivity analysis uses the existing v3 predictions, frozen temperatures and acceptance policies.
It does not retrain models, refit temperatures/thresholds, resplit data, download or inspect images,
or run new model inference. Corrected selected checkpoints may be head-only or fine-tuned; stage/epoch
and checkpoint digests are bound through the Notebook 03 index and Notebook 04 evaluation records.
There are 6 trained runs, 2875 test events, 28 represented test cameras,
and 3 paired training seeds. Manual crop verification was not performed; possible crop-label
mismatches remain a limitation. This is a v3 sensitivity analysis informed by the earlier study,
not a fresh independent test or proof of preregistration. Methods must not be revised using v3 outcomes.

## Camera and category representation
```text
      split  events  represented_cameras largest_camera  largest_camera_events  largest_share  top_three_share  concentration_equivalent_cameras  small_cameras  minimum_events  maximum_events
      train    4377                   64             70                    865        0.19762          0.50902                           9.01014             44               1             865
 validation    1330                   13             34                    289        0.21729          0.56767                           7.00888              6              13             289
calibration    1352                   13            115                    269        0.19896          0.51553                           7.72297              5               4             269
     policy    1389                   13             26                    296        0.21310          0.55436                           7.34596              3              10             296
       test    2875                   28             23                    894        0.31096          0.73843                           4.72922             18               1             894
```
```text
      split  coyote    deer     fox  rabbit   skunk
      train 0.26891 0.26639 0.09390 0.25977 0.11103
 validation 0.37444 0.29023 0.04511 0.25113 0.03910
calibration 0.41642 0.29290 0.04068 0.21450 0.03550
     policy 0.40173 0.27502 0.03672 0.24622 0.04032
       test 0.44939 0.24870 0.03235 0.23896 0.03061
```
Concentration-equivalent cameras is 1/sum(event_share squared), a count-concentration descriptor,
not a statistical effective sample size. Per-category concentration is recorded separately.
The audit describes the already-selected sample; it does not repair camera concentration or establish
that class/background changes caused generalization failures.

## Frozen policy outcomes and abstention
```text
             model variant   status  seeds
   efficientnet_b0     raw selected      3
   efficientnet_b0  scaled selected      3
mobilenet_v3_large     raw selected      3
mobilenet_v3_large  scaled selected      3
```
```text
             model  seed variant policy_status                        policy_reason  total_cameras  cameras_with_acceptance  cameras_without_acceptance                                                                                                                                                          accepting_camera_ids abstaining_camera_ids  accepted  accepted_errors  accepted_error
   efficientnet_b0    42     raw      selected passes_binomial_bonferroni_screening             28                       25                           3                   ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "37", "39", "4", "59", "69", "73", "76", "79", "86", "91", "92", "96"]    ["33", "52", "80"]      1573               12         0.00763
   efficientnet_b0    42  scaled      selected passes_binomial_bonferroni_screening             28                       25                           3                   ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "37", "39", "4", "59", "69", "73", "76", "79", "86", "91", "92", "96"]    ["33", "52", "80"]      1564               10         0.00639
   efficientnet_b0   123     raw      selected passes_binomial_bonferroni_screening             28                       27                           1       ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "86", "91", "92", "96"]                ["80"]      1633               16         0.00980
   efficientnet_b0   123  scaled      selected passes_binomial_bonferroni_screening             28                       27                           1       ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "86", "91", "92", "96"]                ["80"]      1608               16         0.00995
   efficientnet_b0  2026     raw      selected passes_binomial_bonferroni_screening             28                       28                           0 ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]                    []      1985               18         0.00907
   efficientnet_b0  2026  scaled      selected passes_binomial_bonferroni_screening             28                       28                           0 ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "73", "76", "79", "80", "86", "91", "92", "96"]                    []      1976               19         0.00962
mobilenet_v3_large    42     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "59", "69", "73", "76", "79", "86", "91", "92", "96"]          ["52", "80"]      1377                5         0.00363
mobilenet_v3_large    42  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "59", "69", "73", "76", "79", "86", "91", "92", "96"]          ["52", "80"]      1241                3         0.00242
mobilenet_v3_large   123     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1746               12         0.00687
mobilenet_v3_large   123  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1742               12         0.00689
mobilenet_v3_large  2026     raw      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1087                1         0.00092
mobilenet_v3_large  2026  scaled      selected passes_binomial_bonferroni_screening             28                       26                           2             ["102", "104", "109", "122", "125", "13", "135", "23", "25", "27", "28", "29", "32", "33", "37", "39", "4", "52", "59", "69", "76", "79", "86", "91", "92", "96"]          ["73", "80"]      1199                3         0.00250
```
Each seed and camera with no accepted predictions has undefined accepted error, never zero error.
The camera_abstention_summary.csv and test_per_camera.csv files identify the affected cameras and seeds.
Thresholds follow Notebook 04's frozen binomial-bound selection and support requirements.
Camera uncertainty is diagnostic only: zero observed policy errors or an unavailable camera estimate
can coexist with a selected threshold. No-threshold outcomes are not treated as classification failure.
Classification/calibration metrics remain meaningful when no policy accepts.
No camera-weighted result certifies a new threshold or provides guaranteed camera-aware error control.

## Event and equal-camera weighting
```text
             model variant    weighting  accuracy     nll   brier     ece  macro_f1  coverage  accepted_error  seed_count  accepted_error_contributing_seeds  abstaining_seeds accepting_seed_ids abstaining_seed_ids  seeds_with_selected_policy  accepted_error_mean_among_accepting_seeds  accepted_error_std_among_accepting_seeds
   efficientnet_b0     raw equal_camera   0.84819 0.41804 0.21071 0.04808   0.84244   0.59754         0.01010           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01010                                   0.01224
   efficientnet_b0     raw        image   0.87049 0.35569 0.18258 0.02234   0.79448   0.60186         0.00883           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00883                                   0.00110
   efficientnet_b0  scaled equal_camera   0.84819 0.41769 0.21096 0.05025   0.84244   0.59577         0.01035           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.01035                                   0.01278
   efficientnet_b0  scaled        image   0.87049 0.35543 0.18290 0.02110   0.79448   0.59687         0.00865           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00865                                   0.00196
mobilenet_v3_large     raw equal_camera   0.84788 0.43651 0.22039 0.06687   0.84783   0.48680         0.00498           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00498                                   0.00413
mobilenet_v3_large     raw        image   0.88788 0.31792 0.15838 0.02117   0.81425   0.48812         0.00381           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00381                                   0.00298
mobilenet_v3_large  scaled equal_camera   0.84788 0.41238 0.21398 0.04984   0.84783   0.48161         0.00454           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00454                                   0.00428
mobilenet_v3_large  scaled        image   0.88788 0.31151 0.15816 0.02153   0.81425   0.48487         0.00394           3                                  3                 0    [42, 123, 2026]                  []                           3                                    0.00394                                   0.00256
```
Strict all-seed accepted-error means remain undefined if any seed has no acceptance. The separate
conditional mean among accepting seeds reports contributor counts and seed IDs explicitly, matching
Notebook 04's convention. All seeds contribute to coverage and classification/calibration summaries.
Event weighting describes this observed event mixture. Equal-camera weighting draws an observed
camera uniformly and an event uniformly within it; it does not imply representativeness of future sites.
Equal-camera pooled ECE/F1 are computed from camera-normalized additive statistics, not averages of
camera ECE/F1. Weighted accepted error is weighted mistakes divided by weighted accepted observations,
not the mean conditional error among accepting cameras. Those local diagnostics are labelled separately.

## Primary Notebook 04 NLL comparison verified
```text
                                                        comparison metric  matched  parent_estimate  audit_estimate   ci_low  ci_high  valid_replicates  total_replicates
        mobilenet_v3_large NLL gain (raw minus scaled; mean seeds)    nll     True          0.00640         0.00640 -0.00264  0.03936              2000              2000
           efficientnet_b0 NLL gain (raw minus scaled; mean seeds)    nll     True          0.00026         0.00026 -0.01056  0.00914              2000              2000
EfficientNet NLL gain minus MobileNet NLL gain (mean paired seeds)    nll     True         -0.00614        -0.00614 -0.04389  0.00966              2000              2000
```
Event-weighted NLL gains and the architecture difference, including paired bootstrap endpoints and
replicate counts, agree with Notebook 04. This reproduction is a consistency check, not independent
confirmation. NLL gain is raw minus scaled; the architecture contrast is EfficientNet gain minus
MobileNet gain. Positive means EfficientNet improved more; negative means MobileNet improved more.
The equal-camera contrast below is a sensitivity analysis. Brier/ECE are supporting outcomes.

## Camera-specific benefit and weighting sensitivity
```text
             model metric  cameras  improved  worsened  approximately_unchanged  equal_camera_mean_gain
mobilenet_v3_large    nll       28        15        13                        0                 0.02412
mobilenet_v3_large  brier       28        14        14                        0                 0.00641
mobilenet_v3_large    ece       28        10        18                        0                -0.00501
   efficientnet_b0    nll       28        17        11                        0                 0.00034
   efficientnet_b0  brier       28        15        13                        0                -0.00025
   efficientnet_b0    ece       28        17        11                        0                 0.00237
```
```text
                                            comparison    weighting metric  estimate   ci_low  ci_high  valid_replicates  total_replicates
mobilenet_v3_large calibration gain (raw minus scaled)        image    nll   0.00640 -0.00264  0.03936              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image    nll   0.00026 -0.01056  0.00914              2000              2000
                EfficientNet gain minus MobileNet gain        image    nll  -0.00614 -0.04389  0.00966              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled)        image  brier   0.00023 -0.00170  0.00760              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image  brier  -0.00031 -0.00385  0.00260              2000              2000
                EfficientNet gain minus MobileNet gain        image  brier  -0.00054 -0.00997  0.00378              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled)        image    ece  -0.00036 -0.01088  0.01942              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled)        image    ece   0.00124 -0.00947  0.01204              2000              2000
                EfficientNet gain minus MobileNet gain        image    ece   0.00161 -0.02503  0.01989              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera    nll   0.02412  0.00491  0.04567              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera    nll   0.00034 -0.01167  0.01348              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera    nll  -0.02378 -0.04823 -0.00144              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera  brier   0.00641 -0.00040  0.01418              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera  brier  -0.00025 -0.00374  0.00371              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera  brier  -0.00665 -0.01386  0.00060              2000              2000
mobilenet_v3_large calibration gain (raw minus scaled) equal_camera    ece   0.01703 -0.00283  0.02730              2000              2000
   efficientnet_b0 calibration gain (raw minus scaled) equal_camera    ece  -0.00218 -0.01330  0.01165              2000              2000
                EfficientNet gain minus MobileNet gain equal_camera    ece  -0.01921 -0.03344  0.00607              2000              2000
```
Improved/worsened camera counts are descriptive; small-camera estimates are unstable. The
<30-event flag is not an exclusion rule or significance threshold.
Camera IDs represent dataset locations, not verified independent habitats or spatial distances.

## Influence of individual cameras
```text
             model    weighting  minimum_gain  maximum_gain  full_test_gain
   efficientnet_b0 equal_camera      -0.00400       0.00298         0.00034
   efficientnet_b0        image      -0.00457       0.00538         0.00026
mobilenet_v3_large equal_camera       0.01758       0.02770         0.02412
mobilenet_v3_large        image       0.00141       0.01274         0.00640
```
```text
   weighting  cameras  full_test_difference  minimum_omitted_camera_difference  maximum_omitted_camera_difference  sign_flips  direction_changes sign_flip_camera_ids
equal_camera       28              -0.02378                           -0.02751                           -0.01643           0                  0                   []
       image       28              -0.00614                           -0.01731                           -0.00060           0                  0                   []
```
Leave-one-camera-out calculations keep predictions and policies fixed. They show whether omitting
a location changes each model's NLL gain or reverses the comparison between architectures.
A sign flip is descriptive influence, not evidence of significance or a reason to exclude the camera.
The full original test-camera set remains the reported evaluation population.

## Uncertainty and limitations
All analyses inherit 2000 shared whole-camera bootstrap draws with seed
20261026 and 15 confidence bins from Notebook 04. They average the same
3 seed-specific scores inside each draw. Percentile intervals condition on fitted models,
temperatures, policies and this camera split. They exclude refitting/split uncertainty, are exploratory
across supporting outcomes, and do not establish equivalence when they cross zero. Undefined
bootstrap risks remain missing and valid/total replicate counts are reported. Equal-camera weighting
increases the influence of sparsely observed sites; ECE is bin-dependent. Detector pretraining overlap
with Caltech and different classifier pretraining recipes remain limitations. The actual Notebook 04
policy outcomes are retained above; no v1 target-failure conclusion is copied into this report.

## Output files
Representation: camera_concentration_by_split.csv, camera_event_counts.csv, class_camera_counts.csv,
category_camera_concentration.csv, class_distribution_by_split.csv. Performance: test_per_camera.csv,
weighting_comparison.csv, weighting_seed_means.csv, within_camera_diagnostics.csv. Abstention:
seed_acceptance_summary.csv, camera_abstention_summary.csv, frozen_policy_outcomes.csv. Calibration:
notebook04_gain_consistency.csv, camera_calibration_gains.csv, camera_benefit_counts.csv,
weighted_camera_bootstrap_intervals.csv, calibration_gain_contrasts.csv. Influence:
leave_one_camera_out_nll_gains.csv, leave_one_camera_out_summary.csv,
architecture_camera_influence.csv, architecture_camera_influence_summary.csv.
The parent evaluation and its cached predictions remain unchanged.
