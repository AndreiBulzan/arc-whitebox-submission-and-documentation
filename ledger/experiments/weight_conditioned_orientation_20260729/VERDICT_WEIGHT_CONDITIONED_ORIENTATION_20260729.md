# Verdict: weight-conditioned orientation routing

Date: 2026-07-29

Evidence: **component**. All compute and adjusted-score arithmetic is a
**projection**. No physical row, package, upload, submission, or remote action
was performed.

## Bottom line

The eight-orientation atlas contains a large genuine oracle:

| held family | current K146 signed MSE | best S109 pair oracle | ratio | row p95 / max |
|---|---:|---:|---:|---:|
| Full100 | 5.23784e-7 | 1.48705e-7 | 0.2839 | 0.5924 / 0.8858 |
| Generated64 | 6.19177e-7 | 1.94993e-7 | 0.3149 | 0.6237 / 0.9181 |

Every held row improves under that label-using oracle. With all 129 bases per
orientation, the best equal pair reaches 1.26643e-7 Full and 1.71878e-7
Generated. The equal mean of all eight full orientations reaches 1.09798e-7
and 1.96939e-7. The capacity is therefore real, weight-specific, and not a
single-family accident.

The target-free selector does not recover enough of it. The strongest fixed
rule tested uses three full-depth S109 pilot bases in all eight orientations,
a coordinatewise trimmed orientation consensus, and the equal S109 pair
closest to that consensus. It has no target-fitted coefficient. Its signed
MSE is 3.73495e-7 Full and 4.05831e-7 Generated: ratios 0.7131 and 0.6554
against the current proxy, but row-ratio p95 remains 1.697 and 1.692.

Applying that frozen signed correction to the sealed K146 final prediction
through a fixed one-half ReLU derivative gives actual target scores of
1.77459e-7 Full and 1.66852e-7 Generated noise-corrected. These are
first-order component scores, not exact pair-cloud predictions: the atlas
does not retain the complete post-ReLU readout state.

## Fixed Pareto sweep

The same p=3 selector and half-gate were evaluated at every nested S109 prefix;
no support was promoted by target score.

| selected bases / orientation | total unique K | conservative effective projection | Full adjusted | Generated corrected adjusted |
|---:|---:|---:|---:|---:|
| 33 | 84 | 100.37B | 1.447e-7 | 1.117e-7 |
| 49 | 116 | 137.68B | 1.486e-7 | 1.151e-7 |
| 65 | 148 | 174.99B | 1.568e-7 | 1.327e-7 |
| 81 | 180 | 212.29B | 1.663e-7 | 1.340e-7 |
| 97 | 212 | 249.60B | 1.727e-7 | 1.513e-7 |
| 109 | 236 | 277.58B | 1.775e-7 | 1.669e-7 |

The conservative economics scale the 144.013B K146 count and 26.2B remote
residual with K, then add 2.441B fixed overhead. The K236 endpoint is over the
hard budget under this model. More importantly, no point clears 1.2e-7 on
Full, even before implementing the dynamic pilot/main lifecycle.

## Decision

Kill this exact deployment route. Do not build a runtime or spend a physical
row on it.

Retain one research fact: pair identity carries enough per-MLP information to
cut signed endpoint error by about 69--72%, but shallow or tiny full-depth
pilots do not identify the pair reliably enough. A future return is justified
only by a new selector that closes most of the explicit oracle gap at no more
than roughly K116 economics. Repeating pilot sizes, consensus norms, or linear
proxy regressions is not justified; the pilot-only regression recovered just
2.7% Full and 1.0% Generated actual raw improvement.

## Artifacts

- `orientation_oracle_capacity_r0_20260729.json`
  (`b1220c76d9ef5f5c706e0ffde86ade39b7c3f12baf6162d2761625b6e4ab5fbb`)
- `pilot_trimmed_pair_r1_component_score_20260729.json`
  (`42045499cfbff7220be08997e1892d4d4ed6b3eade33a370259132c62e710764`)
- `pilot_trimmed_pair_p3_halfgate_sweep_r2_20260729.json`
  (`8d9dc6a5695cfa7b36c1ebf851eb6ea67e061ec7c32be20e409f5e8d7dd4646e`)

