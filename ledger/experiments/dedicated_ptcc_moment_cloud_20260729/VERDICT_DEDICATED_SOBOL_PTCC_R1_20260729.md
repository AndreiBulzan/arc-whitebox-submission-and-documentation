# Dedicated Sobol PTCC R1 verdict

Date: 2026-07-29

Evidence label: **component**. This was a target-free 2 Full + 2 Generated
offline CUDA capture followed by a separate post-seal score. It used no
FlopScope session, physical estimator row, package, upload, submission, or
remote action.

## Verdict

**Kill this mechanism.** A dedicated small cloud fixes the empirical-law
objection to the production K146 cloud, but it does not estimate the late K4
query accurately enough at the allowed price.

The fixed experiment propagated one 2,048-row scrambled Sobol sphere cloud
with exact antipodes. It shared layers 0--23, then followed both frozen
production late-selection paths. The current K146 mean, variance, and K3
were retained; only the direct fourth cumulant was replaced by the
rank-8 PTCC query.

The declared count upper projection was `9,522,004,352`, including three
late empirical Gram products per arm and a conservative `0.75B` fixed-rank
PTCC allowance. It passes the `10B` price gate.

```text
variant                    Full pooled     Generated corrected   worst row
K4 additive PTCC             6.500604            1.870933          9.518944
K4 additive direct           1.086302            1.130815          1.221535
required                    <=0.880000           <=0.880000         <=1.500000
```

Using the dedicated cloud's K3 as well made the result substantially worse
(`6.729322` Full, `2.086408` Generated corrected). Replacement rather than
additive correction also failed.

The closest lawful member, direct-K4 additive, worsened raw MSE by `8.63%`
on Full and `13.08%` on Generated. Attaching the projected count to the
`171B`, `1.315388e-7` remote K146 anchor gives diagnostic adjusted
projections of `1.5085e-7` and `1.5703e-7`, respectively. Those are
projections, not receipts.

The result rules out a 2,048-row dedicated Sobol late-moment add-on as a path
to the required `>=12%` reciprocal raw gain at `<=10B`. Increasing the cloud
is not justified: the closest member already reverses both families before
paying its positive compute cost, while the PTCC completion is much less
stable than the direct query.

## Artifacts

```text
capture source
  capture_dedicated_sobol_ptcc_r1_targetfree_20260729.py
  985ff21e118fbdd71acf11a1af0f836ba2944a905e24239bc0272fc4e3c96254

target-free capture
  dedicated_sobol_ptcc_r1_targetfree_20260729.npz
  058df83fe713a9921c942d3d7d5f0336399cfbe27d3b49d5c5529f4da6de7cc1

post-seal score
  dedicated_sobol_ptcc_r1_postseal_score_20260729.json
  0ef3ec61ad8eced3b6b45ca5e51b095f801bce397ff45b25c0c582b0cadce9d9
```
