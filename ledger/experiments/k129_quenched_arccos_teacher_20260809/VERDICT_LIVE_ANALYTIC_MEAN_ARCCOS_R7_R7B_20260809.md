# Verdict: live analytic layer-31 mean R7/R7B

## Decision

Reject the paid live analytic closure as a source of the layer-31 mean defect.

## Evidence boundary

**Component** evidence on two Full and two Generated rows. No physical row,
official Mini100 row, remote action, package, upload, or submission occurred.

## Association correction

The original R7 capture used CPU-generated probes and is invalid for scoring:
its beta-zero feature differed from sealed R4 by `2.8224`. R7B regenerated the
same seeded probes on CUDA under the benchmark lock. Its beta-zero association
error is exactly zero and its beta-zero score ratio is `1.0000441002`, exactly
reproducing R4's K129-full result.

## Corrected result

- Primary `beta=1`: ratio `1.0043459846`; family ratios `1.0057403` and
  `1.0020018`.
- Best target-open scalar `beta=-0.5`: ratio `0.7492453845`, but family ratios
  `0.6445143` and `0.9253235`, worst row ratio `1.9422084`.
- Live-versus-dense defect cosines: `[-0.8224, -0.1419, 0.0442, -0.1412]`.
- Live defect norm is `12.78--17.33` times the dense defect norm.

The target-open negative scalar is not a transferable candidate; it reverses
an unstable, badly scaled closure error. The primary construction fails both
the raw and family gates.

## Receipts

- Corrected capture:
  `runtime/artifacts/k129_live_analytic_mean_arccos_r7b_targetfree_20260809.json`
- Corrected postseal:
  `runtime/artifacts/k129_live_analytic_mean_arccos_r7b_postseal_20260809.json`
