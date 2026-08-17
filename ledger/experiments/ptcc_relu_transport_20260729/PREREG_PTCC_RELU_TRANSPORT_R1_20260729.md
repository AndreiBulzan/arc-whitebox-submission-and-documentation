# PTCC ReLU-transport R1 preregistration

Date: 2026-07-29

Evidence scope: teacher-state **component** only.  No FlopScope session,
physical row, package, upload, submission, or remote action is permitted.

## Frozen mechanism

R1 asks whether the rank-8 connected fourth-cumulant state can cross one
nonlinear transition rather than being refit immediately before the query.

For Full rows `0,1,100,101`:

1. Open only radial-demixed post-activation state at layer 29
   (`mean,m2,m3,M11,M21,M22,M31`) and weights `W30,W31`.
2. Fit the already sealed rank-8 PTCC K4 core to layer-29 repeated connected
   views.
3. Compute the exact linear mean/covariance of layer-30 preactivations.
4. Transport inherited connected K4 through ReLU with its analytic first
   derivative `p_i = Phi(mu_i/sigma_i)`.  Equivalently, query the layer-29
   core with `W30 @ diag(p) @ W31`.
5. Add the target-free K4 *birth* produced by rectifying a Gaussian with the
   same layer-30 preactivation mean/covariance.  The birth is evaluated by
   two fixed independently scrambled antithetic Sobol replicates, each
   32,768 points, and averaged.  No coefficient or target fit is allowed.

The capture seals inherited, birth, and summed K4 queries before the scorer
opens final preactivation moments or official means.

## Fixed gate

Treat rows `0,1` and `100,101` as two separate Full development strata.  On
both strata independently, the summed query must:

- reach K4 correlation `>=0.98` and relative RMSE `<=0.15`;
- with exact score-time final K3, reach final post-ReLU MSE `<=1.2e-7`;
- improve exact-K3-only final MSE by at least 25% (`ratio <=0.75`);
- stay finite.

The two Sobol replicate birth queries must have relative RMS disagreement
`<=0.25`.  Failure kills this transport spelling.  Passing licenses a
longer teacher-state rollout and Generated-family state acquisition, not an
estimator or deployment claim.
