# R98 exact odd-linear endpoint correction verdict

**Date:** 2026-08-10  
**Decision:** kill as a candidate; retain only a microscopic positive signal  
**Evidence:** broad statistical official-Mini100 replay; projection for score

The exact decomposition was implemented as preregistered.  Starting from
sealed packaged R92 outputs, the candidate changed only the linear half of
`ReLU(z)=(z+|z|)/2`; the sampled absolute component was left untouched.

Public rows selected `beta=0.004`.  Frozen transfer was:

- public raw reduction: `0.084752%`;
- untouched holdout raw reduction: `0.040921%`;
- pooled raw reduction: `0.060252%`;
- rows improved: `48/100`.

The continuous target-aware scalar capacity is itself tiny: `0.068062%`
public, `0.033364%` holdout and `0.047664%` pooled.  Thus a finer beta grid
cannot turn this into a material correction.

The same-count projection from the observed R92 public aggregate is
`1.1338704237e-7`, but the mechanism fails every preregistered accuracy gate
except finiteness.  Do not package or assign R98.  The result is still useful:
the analytic signed-mean proxy has the same weak positive direction on both
official halves, while bounding this algebraically clean use of it below one
tenth of one percent.

Artifacts:

- target-free candidates:
  `runtime/artifacts/k129_r98_r92_odd_linear_candidates_r1_targetfree_20260810.npz`
- target-free receipt:
  `runtime/artifacts/k129_r98_r92_odd_linear_candidates_r1_targetfree_20260810.json`
- post-seal result:
  `runtime/artifacts/k129_r98_r92_odd_linear_r1_postseal_20260810.json`

No physical, package, upload, submission, or other remote action occurred.
