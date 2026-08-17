# Adaptive compute and blend verdict — R1/R2

Date: 2026-07-29

Verdict: **hard kill** for adaptive routing or scalar arm blending around the
existing frozen K146/K162 endpoints.

Evidence is `broad statistical` for the Full100/Generated64 accuracy tests
and `projection` for union-support compute and remote-score transfer. No
estimator prediction, FlopScope session, physical row, package, network
action, upload, or submission was performed.

## R1: nested-increment support routing

R1 used a genuinely new inference observable absent from the earlier
weight/O0/disagreement routers: the completed convergence increment
`P33 - P17`. S17 and S33 overlap in 16 of S17's 17 bases, so both endpoints
were optimistically priced from a 34-O1-basis union. Larger choices paid for
the literal union with each frozen support plus `0.25B` decision work.

Projected effective prices were:

| chosen O1 support | union O1 bases | projected effective |
|---:|---:|---:|
| 33 | 34 | 186.265B |
| 49 | 53 | 203.045B |
| 65 | 71 | 218.942B |
| 73 | 75 | 222.475B |
| 85 | 86 | 232.190B |
| 97 | 99 | 243.672B |

The target-informed support oracle was strong:

| held family | optimistic oracle adjusted |
|---|---:|
| Full100 | `1.02657e-7` |
| Generated64 corrected | `8.40760e-8` |

The inference signal did not identify that oracle. With model class and
hyperparameters selected only by training-family five-fold score:

| train → held | held adjusted | held best fixed | result |
|---|---:|---:|---|
| Full100 → Generated64 | `1.33786e-7` | `1.14259e-7` | fail |
| Generated64 → Full100 | `1.41531e-7` | `1.42881e-7` | fail |

The scalar extrapolation `P33 + alpha*(P33-P17)` also reversed sign:
Full fitted `alpha=-0.04629`, Generated fitted `alpha=+0.03011`; reciprocal
held adjusted projections were `1.44818e-7` and `1.47795e-7`.

This failure is conservative in the candidate's favor because the price
omits duplicate support-specific repair/readout work.

## R2: conditional K146 arm weight

R2 kept the K146 cloud fixed and learned a per-network scalar O1 weight from
80 coordinate-symmetric O0/O1 endpoint summaries. It charged `0.25B` for
the decision and selected ridge/extra-trees/clipping entirely by
training-family five-fold score.

The target-informed per-row scalar capacity existed:

| family | oracle/literal raw ratio |
|---|---:|
| Full100 | `0.86990` |
| Generated64 corrected | `0.80847` |

But the target-free summaries predicted almost none of it:

| train → held | held raw ratio | remote-calibrated adjusted projection |
|---|---:|---:|
| Full100 → Generated64 | `0.99678` | `1.31307e-7` |
| Generated64 → Full100 | `0.98986` | `1.30395e-7` |

Both miss `1.20e-7` decisively.

## Conclusion

The frozen endpoints contain large *target-informed* rowwise
complementarity, but every available lawful inference observable now gives
the same answer:

- raw weight geometry does not transfer the support decision;
- O0 summaries do not transfer it;
- arm disagreement does not transfer it;
- within-arm jackknife variance does not transfer it;
- the newly tested nested convergence increment does not transfer it;
- conditional O0/O1 blend weights recover only `0.3--1.0%` held-family raw
  gain where roughly `9%` is required.

Therefore adaptive compute is not a route to the `1.2e-7` checkpoint on the
current frozen K146/K162 statistic. Reopening requires a new inference-time
observable that is causally closer to the missing signed final-preactivation
mean—most plausibly a late-layer particle replicate or innovation state—not
another model over final endpoints or supplied weights.

Primary artifacts:

- `nested_increment_router_r1_20260729.json`
- `conditional_arm_blend_r2_20260729.json`
- the two preregistrations and their hash-pinned scorer sources in this
  directory.

