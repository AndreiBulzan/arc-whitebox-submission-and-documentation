# SVD-conditioned K146 secondary orientation R1 — hard kill

Date: 2026-07-29.

Evidence label: **component**. This was an ordinary-CUDA 4 Full + 4
Generated accuracy screen, not broad statistical, FlopScope, measured-whole,
package, or remote evidence.

## Result

The canonical right-singular frame of `W0` does not pass the preregistered
cross-family gate:

```text
                                      Full       Generated corrected
pure SVD / control                 1.095899              0.911399
fixed alpha=0.25 damped / control 0.966426              0.919553
required for pure SVD            <=0.900000            <=0.900000
```

The pure frame worsened Full raw MSE by `9.59%` and missed even the
Generated threshold. The preregistered damped diagnostic improved both
four-row means, but only by `3.36%` and `8.04%`; it is a secondary
cross-mechanism diagnostic and cannot override failure of the pure-SVD hard
gate.

All target-free invariants passed before scoring:

- primary 129-basis arm bit-identical on all eight rows;
- maximum frame orthogonality error `1.8531e-8`;
- predictions finite and sealed before target access;
- shared benchmark lock covered the CUDA capture.

The first post-seal scorer invocation opened the frozen target members and
then stopped on a mechanical Full-target shape mismatch: `targets.npy` is
already `(1000, 256)`, not layer-indexed. The scorer was changed only to map
`targets.npy[rows]` rather than `targets.npy[rows, -1]`; predictions,
methods, rows, coefficient, metric, and hard threshold were unchanged.

## Verdict

Kill this exact SVD/eigenframe orientation without a larger capture,
physical implementation, or coefficient tuning. It does not supply the
required accuracy breakthrough.

## Artifacts

- preregistration:
  `PREREG_SVD_CONDITIONED_QUADRATURE_R1_20260729.md`
  (`8b280c3df909b380246af8a828806197305b3b1e6a0a94e69a8fb2c9b2cbeeaf`)
- capture source:
  `capture_svd_conditioned_quadrature_r1_targetfree_20260729.py`
  (`d5b8d20c01e17f7b1fb9e381adee288e65965092fa440fd6793c34ec9260020b`)
- prediction seal:
  `svd_conditioned_quadrature_r1_targetfree_20260729.npz`
  (`6dcb41cfed61344edb8025a2183409671340052eb85b9f5ab49d79248b4e53be`)
- capture receipt:
  `svd_conditioned_quadrature_r1_targetfree_20260729.json`
  (`6cff831f9862e7b8f28ab48a96de091d57fdc02f3127985090d8e5361074b22b`)
- final scorer:
  `score_svd_conditioned_quadrature_r1_postseal_20260729.py`
  (`7e0718d1ac1c8d24f10eff514bdff521b0f32a6329daf1a0154d57ca5d7776b4`)
- score receipt:
  `svd_conditioned_quadrature_r1_postseal_20260729.json`

No FlopScope session, physical row, package, network action, remote action,
upload, or submission occurred.
