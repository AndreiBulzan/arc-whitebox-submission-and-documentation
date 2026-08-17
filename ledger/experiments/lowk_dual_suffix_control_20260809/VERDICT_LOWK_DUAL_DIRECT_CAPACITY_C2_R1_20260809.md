# Low-K dual direct-support capacity C2 R1 verdict

Date: 2026-08-09

Verdict: **capacity pass; target-free support/weight bridge required**.

Evidence label: component development diagnostic. This is a fixed public16
post-seal capacity study, not Mini100, physical, packaged, or remote evidence.

The literal K20 ordinary plus K20 polar per-basis endpoints associate to the
sealed arm means to `1.776e-15`. With balanced, nonnegative, ridge-regularized
oracle support and weights:

- 12 total tails reach raw MSE `5.8454910372e-7`;
- 14 total tails reach raw MSE `4.4646876216e-7`;
- 16 total tails reach raw MSE `3.4859786671e-7`;
- 20 total tails reach raw MSE `2.2903215315e-7`.

At 14 tails the mean effective sample size is `12.2933`, the minimum is
`11.3913`, and 15/16 rows are below raw MSE `1e-6`. The preregistered capacity
gate therefore passes decisively: exact whole-basis tails are not intrinsically
too weak at the desired floor geometry.

The target-free selectors fail. At 14 tails, checkpoint herding gives
`2.9117571190e-6` and a fixed prefix gives `2.8691563391e-6`. Even oracle
weights on the frozen checkpoint-herding support give only `9.0828521800e-7`.
The largest missing term is therefore support discovery, followed by weight
calibration; it is not suffix propagation capacity.

Next action: capture the layer-5 basis statistics and test a joint ordinary / 
polar sparse calibration that approximates the pooled checkpoint moments under
the same arm-balance, positivity, weight-cap, and effective-sample-size guards.
Only a frozen target-free survivor is eligible for the official Mini100 gate.

Controlling receipt:
`runtime/artifacts/lowk_dual_direct_capacity_c2_r1_postseal_20260809.json`
(`sha256 5e5f30858742a7ed8bbb486f564a16f70a868abb3b9cb518998e8e0f6009b8d0`).

