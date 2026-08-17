# K129 repaired-H1 segmented depth verdict

The component receipt is
`runtime/artifacts/k129_h1_segmented_depth_component_r2_20260731.json`.
It is component evidence only.

The receipt's derived `effective_component_compute` field used the server
budget-close residual rather than the participant-client
`BudgetContext.residual_wall_time_s`. That derived field is invalid for
challenge pricing. The receipt preserves both underlying clocks, so no
physical rerun is needed. Correct repricing is:

| plan | counted | client residual | priced effective |
|---|---:|---:|---:|
| direct | 4,320,067,584 | 0.000016995 s | 4,321,767,084 |
| depth-2 destination | 3,378,339,840 | 0.001155981 s | 3,493,937,940 |
| depth-3 segmented | 3,035,637,760 | 0.007840030 s | 3,819,640,760 |

Depth 2 is the winner. It saves `827,829,144` effective component units
against direct and `325,702,820` against depth 3. It also eliminates the R1
kernel's 24 billed concatenations, stays at `103,563,264` bytes for its
largest result, and has relative RMSE `1.0461e-6`.

Projection, not receipt: holding R26 raw error and remote transfer fixed,
the depth-2 rewrite moves the remote adjusted score from
`1.2004426e-7` to approximately `1.1936e-7`. This does not provide the
20--30% compute reduction needed for a convincing sub-`1e-7` result.

Depth 3 is killed. Depth 2 is promoted only to an estimator-component lead;
it still needs a hash-pinned successor and one measured whole before any
package decision.
