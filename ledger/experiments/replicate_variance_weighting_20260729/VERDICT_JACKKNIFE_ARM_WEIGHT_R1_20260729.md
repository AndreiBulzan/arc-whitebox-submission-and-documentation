# Within-arm jackknife weighting R1 — hard kill

Date: 2026-07-29

Evidence label: **component**.  No physical/timed row, FlopScope session,
package, upload, submission, or remote action was performed.

## Result

The all-eight endpoint atlas does preserve a lawful deployment-computable
uncertainty observable: separate per-basis signed endpoints.  Thus the
earlier static statement that no per-node information exists is too broad.
The exact K162 arms can form a coordinatewise delete-one jackknife variance
without targets, endpoint errors, or cross-arm disagreement.

That observable does not solve the allocation problem under the one
preregistered family-agnostic rule:

| family | candidate / fixed-K162 signed MSE | rows improved | p95 | max |
|---|---:|---:|---:|---:|
| Full12 | `0.9778315` | `58.3%` | `1.03686` | `1.03847` |
| Generated12 | `1.0054655` | `33.3%` | `1.07607` | `1.09174` |

The continuation gate required a ratio at most `0.90` independently in both
families, p95 at most `1.10`, max at most `1.50`, and at least `55%` of rows
improved.  Full misses the gain gate; Generated misses both gain and row
fraction.  Status: **kill**.

## Interpretation

The rule changes weights modestly and has a benign tail, but per-basis
dispersion estimates sampling noise while the important arm error is
orientation-specific deterministic bias.  The sign reversal between Full
and Generated is the same transfer warning seen in prior endpoint-scale
routers.  Do not try trace aggregation, clipping, block counts, covariance
corrections, or weight shrinkage under this lane; those would be
post-outcome variants of the same weak signal.

Artifacts:

- `PREREG_JACKKNIFE_ARM_WEIGHT_R1_20260729.md`
- `jackknife_arm_weight_r1_targetfree_20260729.npz`
- `jackknife_arm_weight_r1_targetfree_20260729.json`
- `jackknife_arm_weight_r1_postseal_20260729.json`

