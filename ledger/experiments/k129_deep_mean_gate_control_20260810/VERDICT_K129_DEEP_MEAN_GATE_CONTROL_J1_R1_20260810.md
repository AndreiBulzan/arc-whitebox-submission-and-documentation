# K129 deep mean-gate control J1/R1 — verdict

**Date:** 10 August 2026  
**Decision:** kill the analytic-mean/mean-gate spelling; retain the surrogate-accuracy specification  
**Evidence:** broad statistical official-Mini100 replay

This was the genuinely distinct part of the external proposal.  Unlike the
recent LMCBR experiment, it did not fit a 129-basis endpoint contrast.  It
transported the sealed analytic-minus-K129 checkpoint mean at layers 23--30
through the supplied weights and a deterministic mean-gate Jacobian.  Both
the analytic Gaussian gate probabilities and literal half gates were frozen
before targets opened.

The result fails decisively:

- analytic-minus-K129 checkpoint discrepancy: roughly `0.0095--0.0098` RMS;
- coefficient-one raw ratios: `2.18x--325.57x` versus R92;
- best continuous pooled scalar capacity over all 16 directions: only
  `0.0572%` raw reduction;
- public-selected cell: layer 23, analytic gates, beta `0.002`;
- public raw reduction: `0.1185%`;
- untouched holdout raw change: **0.4523% worse**;
- pooled raw change: **0.2006% worse**, `45/100` rows improved.

The diagnostic separates operator conditioning from surrogate quality.  The
observed analytic-gate response has order-one amplification, consistent with
the external report's premise.  It still fails because the analytic deep
mean is not two times better than the K129 design defect; its discrepancy is
orders of magnitude too large for the residual being corrected and points
in an almost useless output direction.  The same phenomenon was previously
seen at layer 8; it now holds across every late checkpoint on official
Mini100.

What survives is a precise reopen condition: a materially different
target-free layer-23--30 mean surrogate must first demonstrate, against an
independent high-sample hidden-state teacher, error below roughly half the
K129 checkpoint defect **after the fixed Jacobian action**.  Gaussian
closure, fitted endpoint calibration, and routine scalar shrinkage do not
meet it.

Artifacts:

- target-free capture:
  `runtime/artifacts/k129_deep_mean_gate_control_j1_r1_targetfree_20260810.npz`;
- target-free receipt:
  `runtime/artifacts/k129_deep_mean_gate_control_j1_r1_targetfree_20260810.json`;
- post-seal score:
  `runtime/artifacts/k129_deep_mean_gate_control_j1_r1_postseal_20260810.json`.

No physical row, package, upload, submission, or remote action occurred.
