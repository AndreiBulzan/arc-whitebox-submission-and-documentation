# K129 rolled late omitted-mean stabilizer R1 — verdict

Date: 2026-07-29.

Evidence label: **component development diagnostic** for accuracy and
**projection** for incremental operation count.

## Verdict

**Hard stop. Do not broaden, integrate, or physically benchmark this exact
spelling.**

R1 repaired the precise structural defect in the abandoned Green-function
capture. It did not use a common analytic right/gate chain. At every deployed
K129/O0 late layer `24..30`, each candidate rollout:

1. recomputed `energy_indices` from its own current state;
2. retained the actual 192-coordinate right;
3. transported the exact eight omitted-coordinate sample means through the
   corresponding eight rows of that right;
4. added the shared fold before ReLU; and
5. recomputed the next layer's selection from the corrected state.

The unchanged deployed layer-31 `keep=176` selection and omitted-sample-mean
restoration then ran on the rolled state.

The target-free capture covered 8 Full train, 4 Full development, and 4
Generated MLPs for the sealed grid `beta={0,.25,.5,.75,1}`. Baseline parity
with the frozen K129 `lambda=0.0075` Full0 endpoint was exact:

```text
RMS difference       0
maximum difference   0
```

The path was genuinely rolled: at `beta=1`, `0.244%` of Full and `1.139%`
of Generated stored keep entries changed from beta zero.

## Accuracy kill

The preregistered Full-train selection curve was monotonically worse:

| beta | Full-train candidate / baseline MSE |
|---:|---:|
| 0.00 | 1.000000 |
| 0.25 | 1.002006 |
| 0.50 | 1.004835 |
| 0.75 | 1.008489 |
| 1.00 | 1.012951 |

The frozen rule therefore selected beta zero. Its Full-development and
Generated ratios are exactly `1.0`, failing the required `<=0.92` gate.

After the decision, a descriptive look at the already-sealed nonzero-beta
grid found no hidden checkpoint: every nonzero beta worsened Full development
(`1.000436 .. 1.002571`), while the best Generated ratio was only
`0.992396`. The mechanism is both far below the required 8% gain and
family-inconsistent.

## Economics and boundaries

The deployable increment was projected at `0.25B`, below the `2B` ceiling.
Accuracy killed it before a physical FlopScope price was justified.

- ordinary CUDA capture under `runtime/.benchmark_lane.lock`: `13.032s`;
- peak CUDA allocation: `638,344,192` bytes;
- target-free predictions and every actual keep map sealed before scoring;
- no FlopScope session, physical row, package, network action, upload,
  submission, or remote action.

This closes the exact rolled eight-coordinate **sample-mean** fold. Reopening
requires a new state observable; changing beta, layer subsets, rows, or
post-hoc analytic/sample mixing is not licensed.

## Authoritative artifacts

- `PREREG_K129_ROLLED_LATE_MEAN_FOLD_R1_20260729.md`
- `k129_rolled_late_mean_fold_common_20260729.py`
- `capture_k129_rolled_late_mean_fold_r1_targetfree_20260729.py`
- `k129_rolled_late_mean_fold_r1_targetfree_20260729.npz`
- `k129_rolled_late_mean_fold_r1_targetfree_20260729.json`
- `score_k129_rolled_late_mean_fold_r1_postseal_20260729.py`
- `k129_rolled_late_mean_fold_r1_postseal_score_20260729.json`

