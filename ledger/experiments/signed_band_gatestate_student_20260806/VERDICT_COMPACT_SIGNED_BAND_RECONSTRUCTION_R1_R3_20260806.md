# Compact signed-band reconstruction R1--R3 verdict

Date: 2026-08-06

Evidence label: **component** throughout.  Every teacher and student artifact
was target-free; no challenge target, Mini100 row, FlopScope session, physical
row, package, upload, submission, or remote service was opened.

## Stable teacher confirmation

The unchanged R2 complete-cloud inversion was recaptured on the frozen
production-feature Full20/Generated20 rows with 64 new replicates.  The two
independent 32-replicate low-band corrections correlated at `0.837479`, and
the combined correction RMS was `1.5440245e-4`.  Thus failure of the compact
students below is not explained by an unstable teacher.

## R1: pooled production gate/margin state — killed

The existing 598-dimensional per-neuron production-shaped state was tested
with reciprocal family transfer.  Ridge was selected target-free; GBT was
worse.

```text
direction             teacher MSE ratio   correlation   high-|teacher| sign
Full -> Generated          1.332625          0.043655          0.529688
Generated -> Full          1.004303          0.087529          0.548047
required                  <=0.500000         >=0.650000         >=0.650000
```

Pooling over the 129 bases discards essentially all transferable angular
phase.  Do not retry these summaries with another generic learner.

## R2: per-basis final/L4/L8 means — killed

R2 retained all 129 basis identities in the archived final signed endpoints
and response-projected L4/L8 sequences.  GBT was selected target-free.

```text
direction             teacher MSE ratio   correlation   high-|teacher| sign
Full -> Generated          1.122091          0.112162          0.521094
Generated -> Full          1.027354          0.065146          0.496484
required                  <=0.500000         >=0.650000         >=0.650000
```

Basis-resolved means at these checkpoints are also insufficient.  Reopening
requires within-basis direction state, explicit high-order contractions, or a
derivative/boundary observable—not a wider decoder of the same sequences.

## R3: sparse matched Poisson probes — killed

R3 directly approximated the successful semigroup oracle with two orthogonal,
Latin-stratified directions per basis at all 13 radii.  The fixed primary
probe used 6,708 additional trajectories, `10.15625%` of one K129 population.
Matched q0 centers removed ordinary center-sampling bias, but the four-band
inversion amplified the remaining 516-row-per-radius noise.

```text
primary pooled teacher MSE ratio       2903.4761
primary correction RMS                    8.3510e-3
teacher correction RMS                     1.5440e-4
best of 64 pooled ratios                 2175.0195
fraction of 64 with pooled ratio <=0.8       0 / 64
```

Even averaging all 64 diagnostic corrections leaves teacher-space ratio
`75.83`; the sparse-probe noise is orders of magnitude above the signal.  Do
not spend production engineering on this spelling or select a lucky seed.

## Controlling conclusion

The signed multi-band correction remains real: the separate R2 broad oracle
reduced actual raw MSE by `38.84%` and passed every preregistered family/null/
bootstrap gate.  What failed is cheap observability, not the correction.

The remaining live bridge is genuinely analytic signed-defect transport:
query-directed contractions of ranks 6, 8, 10 and grouped higher bands through
the realized weights and ReLU transitions, with repeated-index power
cumulants retained.  Ordinary covariance repair is already killed in the
capsule, and these receipts close generic decoding plus tiny off-design
sampling.  No estimator implementation is authorized until such a transport
retains at least 70% of the oracle gain and prices below the 15B incremental
ceiling.

## Receipts

- `runtime/artifacts/signed_band_gatestate_teacher_r1_targetfree_20260806.json`
- `runtime/artifacts/signed_band_gatestate_teacher_correction_r1_targetfree_20260806.json`
- `runtime/artifacts/signed_band_gatestate_student_r1_targetfree_20260806.json`
- `runtime/artifacts/signed_band_basis_trajectory_r2_targetfree_20260806.json`
- `runtime/artifacts/sparse_poisson_probe_r3_targetfree_20260806.json`
- `runtime/artifacts/sparse_poisson_probe_r3_correction_targetfree_20260806.json`

