# K146 multilevel width correction R1 — verdict

Date: 2026-07-28

Evidence: **component** target-free CUDA capture followed by the sealed
two-family **component** scorer. All count/effective/adjusted values remain
**projection**. No physical, FlopScope, remote, upload, or submission action
was taken.

## Result

**Kill the exact centered-strata output-level correction.** No primary point
passed. The diagnostic `1/16` points also failed their accuracy/economics
gates.

The capture itself is sound:

- 25 target-free GPU trajectories completed under
  `runtime/.benchmark_lane.lock`;
- the Full0 incumbent trajectory matched its frozen reference exactly;
- the per-basis endpoint mean associated with the ordinary trajectory
  prediction to the preregistered numerical tolerance;
- the prediction archive was sealed before either target dataset was opened.

## Decision table

| low path | high subset | Full raw ratio | Generated raw ratio | projected C B | projected adjusted Full / Generated |
|---|---:|---:|---:|---:|---:|
| s12/w192 | 1/16 | 1.1922 | 1.0165 | 165.661 | 1.518e-7 / 1.294e-7 |
| s12/w192 | 1/8 | 1.0688 | 0.9994 | 173.600 | 1.426e-7 / 1.333e-7 |
| s12/w192 | 1/4 | 1.0339 | 1.0602 | 189.477 | 1.506e-7 / 1.544e-7 |
| s12/w192 | 1/2 | 0.9967 | 0.9982 | 221.231 | 1.695e-7 / 1.697e-7 |
| s07/w184 | 1/16 | 3.1870 | 3.1169 | 149.165 | 3.654e-7 / 3.573e-7 |
| s07/w184 | 1/8 | 2.0095 | 1.5265 | 159.584 | 2.465e-7 / 1.872e-7 |
| s07/w184 | 1/4 | 1.2129 | 1.4540 | 180.421 | 1.682e-7 / 2.016e-7 |
| s07/w184 | 1/2 | 1.0701 | 1.0037 | 222.096 | 1.827e-7 / 1.713e-7 |

Ratios are literal pooled final-layer MSE ratios to the incumbent on the
preregistered four Full and four Generated rows. Projected adjusted values
apply those ratios to remote raw `2.0904670478e-7` at the residual-held
effective-compute projection.

The least bad economical point is `s12/w192 + 1/8`, but even it projects to
`1.426e-7` on Full and `1.333e-7` on Generated and improves only one of four
rows in each family. It is not a checkpoint lead.

The half-basis corrections reconstruct the incumbent prediction closely, but
their `221--222 B` projected effective cost erases the benefit. This is the
central failure: correction sampling error falls smoothly with support size,
and the support needed for incumbent-level accuracy is too expensive.

Target-free correction-vector relative RMSE, median across the eight rows:

| low path | 1/16 | 1/8 | 1/4 | 1/2 |
|---|---:|---:|---:|---:|
| s12/w192 | 0.458 | 0.340 | 0.205 | 0.110 |
| s07/w184 | 0.369 | 0.299 | 0.178 | 0.102 |

There is no small-support variance collapse here. A successor should not
spend more work on evenly sampled endpoint differences. It would need either
to predict the correction from cheap all-basis state, or remove work while
preserving the incumbent statistic directly.

## Artifacts

- `PREREG_MULTILEVEL_WIDTH_CORRECTION_R1_20260728.md`
- `capture_multilevel_width_correction_r1_targetfree_20260728.py`
- `multilevel_width_correction_r1_targetfree_20260728.npz`
- `multilevel_width_correction_r1_targetfree_20260728.json`
- `score_multilevel_width_correction_r1_postseal_20260728.py`
- `multilevel_width_correction_r1_postseal_score_20260728.json`

