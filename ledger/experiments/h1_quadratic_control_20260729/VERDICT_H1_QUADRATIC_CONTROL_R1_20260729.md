# H1 quadratic control R1 — fast kill

Date: 2026-07-29. Evidence label: **component**.

The fixed cross-fitted quadratic control was numerically stable but exposed
essentially no transferable variance signal:

| K | Full ratio | Generated ratio | maximum row ratio |
|---:|---:|---:|---:|
| 16 | `1.008433` | `0.944797` | `1.125747` |
| 32 | `0.995413` | `1.005213` | `1.028627` |

The promotion gate was `<=0.50` pooled in both families and `<=1.25` per
row.  No K passed.  The failure is not instability or cost: predictions
were finite, row tails were bounded, and the incremental arithmetic
projection was below `1B` at K146.  Exact first-layer quadratic state does
not explain the remaining deep quadrature error after H1 repair.

Do not widen the quadratic rank or run a production/physical candidate.
The next scout must use a genuinely non-smooth known-mean feature, not
another polynomial H1 moment.

Authoritative score:
`h1_quadratic_control_r1_postseal_20260729.json`.

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
