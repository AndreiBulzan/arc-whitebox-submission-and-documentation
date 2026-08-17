# K129/O0 jackknife-bias R1 — verdict

Date: 2026-07-29.

Evidence label: **component development diagnostic** for the cache-only
accuracy screen and **projection** for deployment economics.

## Decision

**Stop and seal negative.** No rule met the preregistered `<=0.97` pooled
raw-MSE ratio in both reciprocal family transfers, so no broad replay,
estimator integration, physical meter run, or package work is justified.

The screen evaluated 13 target-free finite-support rules and 48
one-coefficient reciprocal-transfer rules. The best worst-family result was:

```text
rule                      q129 + beta * across-basis standard deviation
beta fitted on Full32                                      0.0149272243
beta fitted on Synthetic32                                 0.0174399854

Full-trained -> Synthetic-held corrected raw ratio          0.9963336824
Synthetic-trained -> Full-held raw ratio                    0.9260701063
worst reciprocal ratio                                     0.9963336824
required                                                    0.9700000000
```

The coefficient sign and magnitude agree, but the useful effect does not:
the Full-held gain is `7.39%`, while the process-separated Synthetic-held
gain is only `0.37%`. Its observed row-ratio p95 is also `1.296` Full and
`1.240` Synthetic. That is not a transferable checkpoint correction.

The next-best rules were still essentially null in the worse family:

```text
crossfit coordinate trim-2 difference       worst ratio 0.996775
crossfit network-mean scale                  worst ratio 0.997048
crossfit network-mean trim-1 difference      worst ratio 0.997407
crossfit centered basis-zero contrast        worst ratio 0.997513
```

## Direct mathematical checks

The classical delete-one endpoint jackknife is the expected exact identity:

```text
Full ratio       1.000000
Synthetic ratio  1.000000
```

First-order prefix Richardson extrapolation is not merely weak; it amplifies
the structured-basis error:

| prefix | Full ratio | Synthetic corrected ratio |
|---:|---:|---:|
| 17 | 1.098919 | 1.412050 |
| 33 | 1.252812 | 1.877762 |
| 65 | 1.738782 | 2.898898 |
| 97 | 4.261572 | 5.523308 |

Every fixed trimmed mean also regressed both families (`1.0094–1.1187`), and
median-of-group-means was much worse (`1.3441–1.5929`). There is no hidden
target-free robust-aggregation candidate in this ladder.

## Interpretation

The 129 O0 basis endpoints behave like exchangeable quadrature replicates
after their nonlinear work is complete:

- deleting one and averaging the pseudo-values contains no bias direction;
- nested-prefix differences are structured noise, not a convergent
  Richardson series;
- dispersion can identify where the estimate is uncertain, but not the
  sign of its error robustly across weight families;
- robust aggregation discards cancellations that the complete Kerdock mean
  needs.

This is consistent with the previous uniform fixed-weight result,
jackknife-variance reversal, supervised support reversal, and failed
endpoint decoders. Reopening this lane requires a new signed upstream
innovation observable; another statistic of the already-paid final O0 basis
population is not supported.

## Scope and evidence

- Historical Full1000/Synthetic1024 O0 basis-response arrays were reused;
  no new trajectory was run.
- The corpus predates the current compressed K129 source. This association
  caveat cannot turn a worst-family `0.9963` result into the required
  `0.97` result.
- Held predictions were serialized and hashed before held slices were
  scored.
- The worst candidate arithmetic is projected below `0.01B`, under the
  declared `0.5B` ceiling; no FlopScope ledger was taken after accuracy
  failed.
- No CUDA run, physical row, package, upload, submission, or remote action
  occurred.

Authoritative artifacts:

- `INVENTORY_AND_PREREG_K129_O0_JACKKNIFE_BIAS_R1_20260729.md`
- `k129_o0_jackknife_bias_tiny_r1_seal_20260729.npz`
  (`ee6a145230cacd7a8abd508efe524d378958fd631e3b68bc0bbaab543ba70e4c`)
- `k129_o0_jackknife_bias_tiny_r1_seal_20260729.json`
- `k129_o0_jackknife_bias_tiny_r1_score_20260729.json`
  (`676013032a6a0d3d6a8c6279fcfc21845d23a692d3bc74d5d39c1bb7adc08d2c`)
- the two hash-pinned reproducer sources in this directory.

