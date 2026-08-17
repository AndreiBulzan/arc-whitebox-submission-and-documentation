# Prefix-telescoping control R1 — hard kill

Date: 2026-07-29

Evidence label: **component**.  Cost remains a **projection**.  No
FlopScope session, physical/timed row, package, upload, submission, or
remote action was used.

## Verdict

Kill the exact coefficient-one L4 depth telescope.  It reduced the
17-basis support error by only about 20%, not the required fivefold, and
its signed-target error is roughly an order of magnitude too large:

| family | telescope/support error ratio | telescope signed MSE | versus full O0 | row p95 |
|---|---:|---:|---:|---:|
| Full200 | `0.781164` | `4.217238e-6` | `7.60853x` | `24.0882x` |
| Generated128 | `0.801390` | `4.199585e-6` | `6.47094x` | `17.3628x` |

The fixed gates were `<=0.20`, `<=5e-7`, `<=1.20`, and `<=2.0`
respectively.  Both families fail every gate.

## Interpretation

The realized nonlinear L4 state plus a mean-gated collapsed suffix is a
real control—the telescope-to-full MSE is below the direct pilot-to-full
MSE in both families—but it leaves almost all of the full-depth
basis-replicate error.  The missing signal is created after L4 by actual
gate-cell transitions, not transported by the mean response.

This controls a seam not covered by Gaussian closure, endpoint PTCC,
sigma-point recubature, or width-multilevel correction.  Its decisive
failure means there is no reason to build its projected `~43 B` 0.9.1
graph.  Reopen only with a coarse suffix that retains actual nonlinear
gate-cell state; moving the same mean response to a nearby checkpoint or
fitting its coefficient would not be a new mechanism.

Authoritative score:
`prefix_telescoping_control_r1_postseal_score_20260729.json`.

