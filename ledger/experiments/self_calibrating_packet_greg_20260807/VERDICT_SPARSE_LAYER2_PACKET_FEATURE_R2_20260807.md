# Sparse exact layer-two packet feature R2 verdict

Date: 2026-08-07

## Verdict

**Kill exact first-layer absolute state plus up to 32 sparse exact layer-two
probe neurons.  Do not run a cross-fit or add more layer-two probes by routine
sweep.**

The target-free optimal output projection failed on every Full and Generated
row.  This licenses only a deeper actual-network shadow/progressive oracle.
No targets, FlopScope session, physical row, estimator code, production price,
package, upload, or submission were used.

## Results

All variants used the complete `m=8` packet population and an optimal
network-specific linear map to the full candidate-output deviations:

| Feature span | Rank | Pooled variance explained | Residual selection variance |
|---|---:|---:|---:|
| exact `abs(z1)` | 256 | `11.658%` | `1.8882e-7` |
| plus 8 exact L2 probes | 280 | `11.984%` | `1.8812e-7` |
| plus 16 exact L2 probes | 304 | `12.244%` | `1.8756e-7` |
| plus 32 exact L2 probes | 352 | `12.751%` | `1.8648e-7` |

The frozen gates required at least `70%` explained variance and residual
variance at most `4.0e-8`.  With 32 probes, the best row reached only
`19.22%`, the median was `9.53%`, and `0/16` rows reached even `60%`.

The probes add real but very weak incremental information: 32 exact neurons
improved pooled explained variance by only `1.09` percentage points over the
full 256-dimensional first-layer absolute state.  This is too shallow an
observable, not an optimizer or calibration problem.

One preregistered probe-product column was identically zero on three network
variants.  It was retained as zero without replacement, reducing the literal
effective rank.  This cannot account for the failure: the strongest variant
missed the required variance reduction by about `5.5x`.

## Controlling interpretation

Combined with R1:

- universal NNGP span: `2.7%`;
- exact H1-conditioned NNGP span: `7.3%`;
- complete `abs(z1)` state: `11.7%`;
- complete `abs(z1)` plus 32 exact L2 probes: `12.8%`.

Useful candidate-specific information appears only after materially deeper
interaction with the supplied finite network.  The next oracle should measure
control-variate capacity of complete pair states at increasing depths before
building a shadow network.  Candidate-label agreement remains irrelevant;
the metric is exact residual selection variance.

## Receipts

- `PREREG_SPARSE_LAYER2_PACKET_FEATURE_R2_20260807.md`
- `capture_sparse_layer2_packet_feature_r2_targetfree_20260807.py`
- `runtime/artifacts/sparse_layer2_packet_feature_r2_targetfree_20260807.npz`
- `runtime/artifacts/sparse_layer2_packet_feature_r2_targetfree_20260807.json`

Evidence label: target-free **component** on a reused 16-network bank.
