# Conditional width-200 reconstruction R1 — verdict

Date: 2026-07-29

## Result

Killed at the preregistered two-Full/two-Generated pilot.  The remaining
four rows were not run.

The compute projection passed:

```text
width-200 base effective, residual held     161.296B
conditional reconstruction overhead           0.095B
candidate effective projection              161.391B
fast-falsifier ceiling                       165.000B
```

Accuracy did not:

| family | mean-fold w200 / w216 | conditional w200 / w216 | loss recovered |
|---|---:|---:|---:|
| Full | 1.09238 | 1.14786 | -0.60051 |
| Generated | 0.96576 | 1.07268 | not positive |

The conditional map therefore amplified the Full width-reduction loss and
reversed the Generated width-200 gain.  Its worst row ratio was `1.45833`.
The preregistered Full recovery requirement was `>=0.70`, while the measured
value was `-0.60051`.

The analytic diagnostic explains why this was unlikely to recover the
trajectory: across the sampled layer/row maps, retained coordinates explained
only a small fraction of omitted analytic variance (typically well below
five percent).  The target-free covariance is almost diagonal in the selected
basis, so a dense linear conditional map carries little useful omitted
fluctuation and its accumulated perturbation hurts the nonlinear trajectory.

Evidence label: `component` accuracy and `projection` cost.  Predictions were
sealed before the already-open targets were accessed.  No FlopScope row,
package action, network action, upload, or submission was performed.

## Artifacts

- `PREREG_CONDITIONAL_W200_R1_20260729.md`
- `capture_conditional_w200_r1_targetfree_20260729.py`
- `conditional_w200_r1_pilot_targetfree_20260729.{npz,json}`
- `score_conditional_w200_r1_postseal_20260729.py`
- `conditional_w200_r1_pilot_postseal_score_20260729.json`

