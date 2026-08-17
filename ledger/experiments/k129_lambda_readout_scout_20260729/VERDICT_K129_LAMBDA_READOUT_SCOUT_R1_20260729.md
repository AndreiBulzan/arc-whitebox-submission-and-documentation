# K129/O0 lambda readout scout R1 — no-go

Evidence boundary: the three literal endpoint arrays and their named scores
are **broad statistical** evidence. Values between `0`, `0.0075`, and `0.015`
are piecewise-interpolated **projections**, not new literal captures. This
scout ran no FlopScope row, package, upload, or remote action.

## Result

The zero-count fixed-lambda lane is exhausted. Keep `lambda = 0.0075`.

| fit family | continuous optimum | extra raw gain vs `0.0075` |
|---|---:|---:|
| Full100 | `0.00877277` | `0.015653%` |
| Generated64 corrected | `0.00598212` | `0.022035%` |

The family optima lie on opposite sides of `0.0075`. Training on one family
and applying to the other reverses:

- Full optimum on Generated64: `-0.052446%`;
- Generated optimum on Full100: `-0.059595%`.

The equal-relative-family compromise is `0.00738456`; it improves Generated
by only `0.003224%` while worsening Full by `0.002968%`. Thus there is no
positive same-lambda robust gain. Even the best family-specific gain is
`18.15x` short of the required additional `0.4%` rank-49 rescue.

Literal midpoint curvature is only `4.04e-5` and `4.14e-5` of the first
endpoint step in RMS, so endpoint nonlinearity does not hide a materially
better interior optimum.

## Held-out and adaptive checks

Position-mod-5 fitting worsens its held rows:

- Full100: `-0.35343%`;
- Generated64 corrected: `-0.92159%`.

A target-using per-MLP oracle retains `3.01%` Full and `3.94%` corrected
Generated capacity, but it is not deployable: `76.0%` and `81.25%` of row
optima land on an endpoint boundary, and the maximum absolute correlation
with the small target-free endpoint-feature census is only `0.180` and
`0.121`.

There is also no strictly zero-graph adaptive decision surface in the
production readout. Lambda is consumed to form the correction before the
readout features and gamma exist. The available pre-lambda quantities are
vectors (`proxy_ez`, `sample_ez`, and `delta_unit`); converting them into a
per-MLP scalar requires new reductions/control and is not a zero-count
constant change. The richer retrospective response/curvature features would
require evaluating alternative endpoints and still do not clear `0.4%`.

## Decision

- Keep R21's `lambda = 0.0075`.
- Do not implement another fixed-lambda successor.
- Do not spend a physical row or packaging pass on this lane.
- The checkpoint must come from another statistic or compute contraction.

Reproducer:
`run_k129_lambda_readout_scout_r1_20260729.py`
(`eaffbf61d2ca4d169739b9e865b17c659673ce7922fb3d07dc1c4d2cbe96cc2d`)

Receipt:
`k129_lambda_readout_scout_r1_20260729.json`
(`8fc3b1e60dd1da543ff20aae77d3078c3210da800506690da2bb06c7fc23506e`)
