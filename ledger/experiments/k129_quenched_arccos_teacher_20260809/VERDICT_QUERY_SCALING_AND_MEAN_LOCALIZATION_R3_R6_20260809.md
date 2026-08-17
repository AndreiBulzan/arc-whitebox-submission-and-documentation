# Verdict: query scaling and layer-31 mean localization (R3--R6)

## Evidence boundary

All results below are **component** evidence on two Full and two Generated rows. No official Mini100 prediction, physical FlopScope session, remote action, upload, or submission was performed.

## Decision

The high-capacity quenched last-layer arc-cosine representation survives. Generic query quadrature at a score-compatible row count does not. The remaining sufficient statistic has been localized to the 256-dimensional layer-31 activation mean defect.

## Receipts

- R3 postseal: `runtime/artifacts/k129_query_design_scaling_r3_postseal_20260809.json`
- R4 postseal: `runtime/artifacts/k129_gaussian_moment_arccos_r4_postseal_20260809.json`
- R5 postseal: `runtime/artifacts/k129_recursive_covariance_arccos_r5_postseal_20260809.json`
- R6 postseal: `runtime/artifacts/k129_mean_defect_query_r6_postseal_20260809.json`

## R3: query scaling

The artifact status `targetfree_pass` must not be read as a production pass. Its best frozen Kerdock result used 129 complete bases in each of two replicas (132,096 extra rows) and reached ratio 0.601114. At affordable counts, target-free Haar and Kerdock rules were harmful. Target-open scaling confirms convergence, but too late:

- Haar 64 frames per replica / 65,536 rows: ratio 0.589975.
- Kerdock 129 bases per replica / 132,096 rows: best target-open ratio 0.447418.

R3 therefore kills brute-force query duplication, not the teacher representation.

## R4: Gaussian moment capacity

Using near-exact dense layer-31 moments in the arc-cosine readout:

- full covariance: ratio 0.403780 (59.62% raw reduction);
- diagonal covariance: ratio 1.000220;
- K129 full moments: ratio 1.000044;
- K129 diagonal moments: ratio 1.000608.

Every row improved under the dense full-moment construction. This establishes that a Gaussian moment representation is sufficient and that cross-neuron covariance matters.

## R5: exact localization

The diagnostic substitutions resolve which dense moment matters:

- dense mean + K129 covariance: ratio 0.401912;
- K129 mean + dense covariance: ratio 1.000045.

Thus K129's full covariance is already sufficient for this readout; the missing object is the dense layer-31 mean. Direct recursive full-covariance and Hermite-1 closures were neutral or harmful (best recursive ratio above 1), so those mean recursions are closed.

## R6: mean-only queries

The best score-compatible-size target-open candidate was Haar16 per replica (16,384 total rows), shrink 0.75, relative ridge 0.003:

- pooled ratio 0.817642 (18.24% raw reduction);
- family ratios 0.840888 and 0.778560;
- worst row ratio 1.070251.

The target-free selector for that family had ratio 3.848008. Larger designs improve, but require an uneconomic fraction of another full cloud and remain family-unstable.

## Next gate

Keep the K129 full covariance and arc-cosine feature/readout machinery fixed. Test only cheaper target-free sources of the layer-31 mean defect:

1. the live R90 analytic closure's already-paid layer-31 mean;
2. intermediate full-covariance packet/Gaussian-smoothing corrections;
3. only if one survives, broaden beyond component evidence before production implementation.
