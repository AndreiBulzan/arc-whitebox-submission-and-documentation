# Production K146 rolled-state stabilizer R2 — verdict

Date: 2026-07-29

Evidence label: **component development diagnostic** for accuracy and
**projection** for incremental operation count.

## Verdict

**Hard stop. Do not broaden, integrate, or physically benchmark this exact
spelling.**

R2 ran the requested new observable rather than another detached endpoint
head: the actual K146/m17 per-arm/per-basis state was corrected at eight
checkpoints, and every later fit saw the candidate's own rolled state.
The unmodified path reproduced the frozen K146 CUDA adapter bit-for-bit.

The fixed global shrink grid selected `eta=0.50` on Full development:

```text
eta                         0.00      0.25      0.50      0.75      1.00
Full-dev MSE ratio          1.000     0.91995   0.90713   0.90991   0.92589
```

Predictions were then sealed before either held family was opened. The
development gain reversed independently in both families:

| sealed pilot bank | pooled ratio | observed row-ratio p95 | rows improved |
|---|---:|---:|---:|
| Full held `16..19` | `1.057951` | `1.626807` | `25%` |
| Generated held `0..3`, noise-corrected pooled | `1.112804` | `1.123504` | `25%` |

The preregistered requirements were pooled ratio `<=0.88` and p95
`<=1.25` in both families. Full failed both; Generated failed the pooled
gate. This is a family-consistent reversal, not a marginal miss.

## Interpretation

The actual production basis contrasts contain enough fit variance to make a
four-network development block look about 9% better, but the learned
direction does not predict the target-aligned K146 quadrature error. Rolling
the correction through the real operation graph does not rescue that lack
of transferable signal. This is consistent with, but stronger than, the
older detached response-projection negative because R2 used current K146
states and closed-loop propagation.

The prospective implementation was inexpensive: a conservative
`0.500B` incremental-count projection, below the `2B` ceiling. Accuracy
killed it before a physical price was justified.

## R1 audit

The first execution compared unit-sphere cloud means directly to radial
`official_alm` targets. It selected eta zero before any held access and is
invalid as a method result. R2 changed only the required
`official_alm / chi_mean(256)` unit conversion; no feature, checkpoint,
split, ridge, shrink grid, or gate changed. See
`R1_INVALIDATION_UNIT_AUDIT_20260729.md`.

## Boundaries

- ordinary CUDA only, coordinated through `runtime/.benchmark_lane.lock`;
- no FlopScope session or physical row;
- no package, upload, submission, network, or remote action;
- four held rows per family make this a kill diagnostic, not broad
  statistical evidence.

## Authoritative R2 artifacts

```text
PREREG_PRODUCTION_K146_DAGGER_PILOT_R2_20260729.md
  937f6c95600fb9f39bbef05fb18535938282d474811d0d3e07d74e5b503e50f5

run_production_k146_dagger_pilot_r2_20260729.py
  b38d057490d2a6e44100884310786de5a2293f2631bc23c1b1e123afbb8411b0

production_k146_dagger_pilot_r2_seal_20260729.npz
  83cc8209a71373d10d2f20eeffa5ffa86305e7d36bafff4fd1a5ced00a7a7da7

production_k146_dagger_pilot_r2_seal_20260729.json
  aa0898203699ffedba0a4fbb16dfff723187919c61536bb978f46cf0163af1a8

score_production_k146_dagger_pilot_r2_20260729.py
  d4a759e2502cf9857d14dbc38259d325b8f12eff6407d61ac66d47a06d45b24b

production_k146_dagger_pilot_r2_score_20260729.json
  513e91e3d48832294d5b1b4e4b9dda613e3fdc151433a232dbd15744805510d5
```
