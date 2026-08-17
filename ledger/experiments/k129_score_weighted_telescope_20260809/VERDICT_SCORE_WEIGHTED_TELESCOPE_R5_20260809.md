# Score-weighted nested continuation telescope — R5 verdict

Date: 2026-08-09.

## Decision

**Reject a universal score-weighted continuation schedule on the current
nested K129 endpoint family.** The new weighted-barycenter identity is exact,
and it does remove the unknown cheap/full cross term that killed ordinary R4
randomized continuation. Its transferable capacity is only about `0.54%`,
far below the promotion gate, and its suite p95 is worse than always-full.

This is **broad statistical** development evidence plus a **projection**:
Full100 and Generated128 targets were historically opened, while branch
compute multipliers remain modeled. No Mini100 row, physical row, FlopScope
session, package, upload, submission, or remote action occurred.

## Exact result

The best safe (`Amax<=2`) reciprocal-family constructions both used levels

```text
64, 80, 96, 112, 120, 129
```

and target-free increment-Gram coefficient optimization. The identity checks
passed below `1e-10`.

| train → frozen test | train ratio | test ratio | projected from R87 | test p95 |
|---|---:|---:|---:|---:|
| Full → Generated | 0.994043 | 0.994548 | 1.137093e-7 | 1.01032 |
| Generated → Full | 0.994092 | 0.994635 | 1.137193e-7 | 1.01788 |

The pooled reciprocal ratio is `0.994591`, projecting to
`1.1371428e-7` from banked remote R87. The looser `Amax<=4` search selected
the same solutions, so coefficient headroom is not controlling.

## Interpretation

For each terminal branch, the construction enforces

```text
sum_k pi_k q_k Z_k = Q F.
```

Consequently its expected official loss is exactly

```text
Q MSE(F,truth) + sum_k pi_k q_k ||Z_k-F||^2/256.
```

That is a real improvement over R4: the unknown error/discrepancy alignment
is absent. The remaining failure is economic. K129 prefix increments shrink
too slowly relative to their marginal basis cost; nearly all useful mass
stays at the full endpoint, leaving only a half-percent score gain and
unacceptable suite variance.

Close universal two-level and multistage score-weighted continuation on
these prefix means. A row-conditioned successor would again need a reliable
target-blind full-error observable, the same object already closed by the
difficulty-router program.

## Artifacts

- preregistration SHA-256:
  `98f170dd2838aebe96d384193c0a777e5eb69ae87ef62dc1b12e969328441850`
- source SHA-256:
  `1aac48f7f96ff855dda94556a3b950c8558796149fa928f892d9c34e5ab9f473`
- receipt SHA-256:
  `d544c3ad134ba104033db11b47c8dd32c24b18e0ed3bd89796947b2370163e21`
- receipt:
  `runtime/artifacts/k129_score_weighted_telescope_r5_postseal_20260809.json`

