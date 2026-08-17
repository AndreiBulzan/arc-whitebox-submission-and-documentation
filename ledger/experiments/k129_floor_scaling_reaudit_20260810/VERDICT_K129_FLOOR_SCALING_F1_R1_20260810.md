# K129 floor-scaling re-audit F1/R1 — verdict

**Date:** 10 August 2026  
**Decision:** the random low-K floor is not a free score match  
**Evidence:** broad statistical official-Mini100 finite-population risk; compute and adjusted score are projections

The exact without-replacement risk was evaluated from the already sealed
100×129×256 endpoint tensor.  Over K=12--50, the descriptive raw-MSE slope is
indeed `p=1.05144`.  That confirms the roughly inverse-K regime, but puts this
design on the report's own `p>1` side.

More importantly, complete-K129 cancellation creates a lower full-design
error than a naive `MSE_129 * 129/K` extrapolation.  Under the deliberately
optimistic linear cost model anchored at R92 remote compute:

| K | pooled expected raw MSE | projected multiplier | projected adjusted |
|---:|---:|---:|---:|
| 25 | `1.65956e-6` | `0.1` | `1.65956e-7` |
| 32 | `1.27899e-6` | `0.12737` | `1.62899e-7` |
| 50 | `7.89691e-7` | `0.19901` | `1.57155e-7` |
| 96 | `3.72879e-7` | `0.38210` | `1.42476e-7` |
| 129 | `2.56981e-7` | `0.51344` | `1.31945e-7` |

K25 is `25.78%` worse than K129 on this same-statistic projection.  Public
and holdout agree in sign.  This calculation is exact for a uniform random
K-basis support; it does not rule out a new target-free nonuniform design,
but earlier broad support-selection experiments supply no such survivor.

The report correctly identified the flat-law question as decisive.  It was
not, however, an overlooked win: this law and its execution/statistics
decomposition were already present in the capsule, and the official-Mini100
re-audit selects the large-K side.

Authoritative receipt:
`runtime/artifacts/k129_floor_scaling_f1_r1_postseal_20260810.json`.

No physical row, package, upload, submission, or remote action occurred.
