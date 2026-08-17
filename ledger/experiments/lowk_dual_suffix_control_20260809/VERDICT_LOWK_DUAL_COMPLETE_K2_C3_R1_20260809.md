# Low-K dual complete-K2 C3 R1 verdict

Date: 2026-08-09

Verdict: **kill complete-K2 joint herding at layers 5--6**.

Evidence label: component development diagnostic on fixed public16. No
Mini100, physical, packaged, or remote claim is made.

The capture retained all 40 complete-basis prefixes and formed target-free
feature Gram matrices for:

- layer-5 first and complete second moments;
- layer-6 actual preactivation first/second moments;
- layer-6 actual ReLU first/second moments;
- the layer-6 preactivation/ReLU cross moment.

Support was selected jointly in ordinary/polar pairs. Every arm retained mass
1/2; selected weights were nonnegative, ridge-regularized, capped at twice the
within-arm uniform weight, and had high effective sample size.

The best admissible result used the full layer-6 algebra with seven bases per
arm. It reached raw MSE `2.1405917462e-6`, far above the preregistered
`1.0e-6` gate. Layer-5 mean/diagonal-only reached `2.4491138844e-6`; adding
the complete off-diagonal layer-5 covariance worsened it to
`2.6370271086e-6`. Thus complete covariance contains a little useful ordering
information only after the supplied-weight/ReLU step, but not remotely enough
to recover C2's `4.4646876216e-7` 14-tail oracle capacity.

This closes this exact moment-kernel/herding spelling. It does not close direct
suffix pilots, a learned checkpoint-to-support map, or the external friend's
implementation. The next cheapest nonlearning gate is direct endpoint
compression: exact output-kernel herding can approximate the full 40-tail
estimator at 14 tails with raw MSE `1.0721138128e-6` and at 16 tails with
`1.0052606445e-6`. That is close enough to justify testing a few exact suffix
pilot pairs per basis as a model-assisted residual control, but it leaves
almost no floor margin.

Controlling receipt:
`runtime/artifacts/lowk_dual_complete_k2_c3_r1_postseal_20260809.json`.

