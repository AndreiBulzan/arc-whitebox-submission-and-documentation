# Verdict: learned orientation selector R1

Date: 2026-07-29

Evidence: **component** on a whole-MLP group split of the existing signed
endpoint atlases.  K84 economics are a **projection**.  No physical row,
FlopScope session, package, upload, submission, or remote action was
performed.

## Bottom line

**Kill this spelling.**  One fixed shared ridge L-estimator did not recover
the large S109 pair oracle and was worse than the existing hand selector on
both untouched families.

| S109 selector | Full100 aggregate ratio | Full p95 | Generated64 ratio | Generated p95 |
|---|---:|---:|---:|---:|
| label oracle | 0.2839 | 0.5924 | 0.3149 | 0.6237 |
| hand trimmed consensus | 0.7131 | 1.6974 | 0.6554 | 1.6918 |
| learned target ridge | 0.7721 | 1.9549 | 0.7118 | 1.7881 |

The promotion requirements were `<=0.50` aggregate ratio and `<=1.50` p95
on both families, in addition to materially improving the hand rule.  All
three gates failed.

## What was actually tested

- Training: the 100 Full200 atlas MLPs outside the incumbent K146 Full100.
- Tests: untouched incumbent Full100 and Generated64.
- Generated fitting or selection: none.
- Features: the first three bases of the frozen S109 support in all eight
  orientations, `8 x 3 x 256`.
- Model: one 24-feature standardized ridge with fixed strength `0.01`.
  Orientation triples are sorted by their local three-basis mean before
  flattening, making the feature invariant to orientation relabeling.  The
  same coefficients act independently on every output coordinate, making
  the estimator output-coordinate permutation equivariant.
- Searches or alternative model spellings: zero.

The learned target vector itself was slightly better than the hand
consensus:

| family | learned target MSE / hand target MSE |
|---|---:|
| Full100 held | 0.9645 |
| Generated64 transfer | 0.9522 |

That modest denoising did not translate into better discrete pair routing.
Learned/oracle pair identity agreement was only `5.0%` Full and `3.125%`
Generated.

## Stronger K84 obstruction

At the proposed K84 support—three pilots in all orientations, then 33 total
bases in the selected pair—even a label-using best-pair oracle has signed-MSE
ratio `0.9833` Full and `0.8875` Generated.  Thus better pair classification
alone cannot produce checkpoint-scale accuracy at S33.  The S109 oracle
capacity appears only after buying substantially more support in the chosen
orientations; it is not latent K84 capacity waiting for a smarter selector.

Do not tune this ridge, add a nonlinear classifier, or spend a physical row
on the route.  Reopening requires a new mechanism that makes the selected
pair accurate at low support, not another way to classify the same noisy
three-basis pilot.

## Artifacts

- `PREREG_LEARNED_ORIENTATION_SELECTOR_R1_20260729.md`
- `screen_learned_orientation_selector_r1_20260729.py`
- `learned_orientation_selector_r1_20260729.json`

