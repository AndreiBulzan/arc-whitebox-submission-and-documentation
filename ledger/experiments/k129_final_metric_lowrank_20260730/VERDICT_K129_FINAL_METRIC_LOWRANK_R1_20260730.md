# K129 final metric-low-rank R1 verdict

## Verdict

**Kill and stop.** The fixed final-only covariance-metric rank-8
replacement misses the preregistered `candidate/control <= 0.97` raw-MSE
gate on both families:

| family | rows | corrected control MSE | corrected candidate MSE | ratio | gain |
|---|---:|---:|---:|---:|---:|
| Full | 4 | `2.5715740961e-7` | `2.5681642367e-7` | `0.998674019` | `0.133%` |
| Generated | 4 | `2.1720151742e-7` | `2.1708172957e-7` | `0.999448494` | `0.055%` |

This is a **component development diagnostic**, not broad statistical or
remote evidence. It is roughly 20--55 times too small for the fixed 3% raw
gain gate and has no useful conservative remote projection.

## What was established target-free

- Control and candidate both have 176 primary final features, exact full
  affine mean restoration, `lambda=0.0075`, and scale `1.000025`.
- The metric construction's centered affine-product squared Frobenius error
  was no worse than control on all eight sealed rows. Ratios were
  `1.0000, 0.9553, 0.9656, 0.8857` on Full and
  `1.0000, 0.8901, 0.8404, 0.9122` on Generated.
- Numerical covariance ranks were only `6,18,19,32` on Full and
  `6,32,15,22` on Generated. The first row in each family therefore used the
  preregistered target-free coordinate-continuation stability fallback and
  produced prediction identity.
- Even material affine-product improvement usually vanished at prediction
  scale. Five of eight prediction RMS deltas were below `9e-7`; the two
  largest were still only `3.35e-5` and `8.42e-6`.

The structural result is that improving final compressed affine
reconstruction is not a sufficiently coupled objective for this estimator's
final MSE. The deep cloud is often numerically low-rank, and the existing
mean-preserving final176 map has already removed almost all actionable error
on this surface.

## Seal history and boundary

Two target-free implementation attempts stopped before artifact creation:
the literal inverse-square-root form encountered rank deficiency, and the
first stable-SVD form missed literal dominance only at roundoff scale
(`~1.9e-28` versus control `~1.1e-29` squared error). The stability fallback
was then added to the preregistration. The resulting source/preregistration
became the **first and only** R1 seal; no sealed artifact was overwritten.
Only after its hashes and target-free boundary were verified were the fixed
Full4 and Generated4 targets opened.

No FlopScope session, physical row, package, upload, submission, or remote
action was performed.

Authoritative artifacts:

- `PREREG_K129_FINAL_METRIC_LOWRANK_R1_20260730.md`
- `k129_final_metric_lowrank_r1_targetfree_20260730.json`
- `k129_final_metric_lowrank_r1_targetfree_20260730.npz`
- `k129_final_metric_lowrank_r1_postseal_score_20260730.json`
