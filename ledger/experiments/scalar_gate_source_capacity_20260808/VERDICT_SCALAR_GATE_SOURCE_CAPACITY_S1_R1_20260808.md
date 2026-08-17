# Scalar gate-source capacity S1/R1 verdict

Status: **independent scalar-source sampling at 5,000 transports is rejected;
collective sources remain open**.

Evidence label: stratified 516-line **component** estimate on Full 640--641
and Generated 88--89, with 64 packet replicates and 64 downstream-Jacobian
probes.

- The pooled scalar source-norm sum is `54.208`, versus pooled net source
  correction norm `0.264`.
- Even oracle-probability `S=5000` sampling adds `2.359e-3` pooled MSE.
- The broad 35%-gain allowance is `6.248e-8`; the estimate is about 37,800
  times larger.
- The bootstrap 5th percentile is `2.342e-3` and all noise/probe halves fail
  by similar margins.
- The exact target-free secant identity check passed: maximum trajectory
  identity error `1.05e-6`, antithetic tangent `1.19e-8`, and pair-source
  error `8.87e-8` in float32.

This is the quantitative cancellation/sign obstruction.  It rejects the
expert's independent scalar importance sampler at the stated budget, not
layer-grouped, correlated, stratified, or deterministic source estimators.

Receipts:

- `runtime/artifacts/scalar_gate_source_capacity_s1_r1_targetfree_20260808.json`
- `runtime/artifacts/scalar_gate_source_capacity_s1_r1_postseal_20260808.json`
- `runtime/artifacts/scalar_gate_source_identity_r1_20260808.json`

