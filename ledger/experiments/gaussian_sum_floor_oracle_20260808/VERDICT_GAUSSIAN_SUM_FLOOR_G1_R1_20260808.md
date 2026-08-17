# Gaussian-sum floor G1/R1 verdict

Status: **tested teacher grid has no survivor; not a universal Gaussian-sum
class kill**.

Evidence label: **component** on Full 640--641 and Generated 88--89.  All
costs below are projections.

- The best random-mixture ceiling was `2.0791e-6` noise-corrected pooled raw
  MSE (`K=20`), already above the `1e-6` score-floor requirement.
- The best one/two-basis local cubature was `1.1381e-4` pooled raw MSE
  (`K=20`, two bases), at a projected `26.84B` leading count.
- No preregistered configuration passed the pooled `7.5e-7`, per-family
  `1e-6`, and `<=24B` gates.

This rejects state-only MiniBatch-K-means full-covariance teachers at layer
24 with the tested axial/Hadamard local cubatures.  It does not prove that an
output-aware adaptive split/merge mixture is impossible.  However, both the
mixture ceiling and the local cubature fail independently, so recursive
splitting is not licensed from this result.

Receipts:

- `runtime/artifacts/gaussian_sum_floor_g1_r1_targetfree_20260808.json`
- `runtime/artifacts/gaussian_sum_floor_g1_r1_postseal_20260808.json`

