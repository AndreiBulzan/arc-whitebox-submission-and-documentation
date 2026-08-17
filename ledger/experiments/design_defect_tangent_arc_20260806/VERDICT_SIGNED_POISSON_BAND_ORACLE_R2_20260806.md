# Verdict: target-free signed Poisson-band oracle R2

Date: 2026-08-06

Status: **PASS signed-band capacity; transport and production remain unproved**

Evidence label: **broad statistical** for the fixed Full8/Generated8
post-seal result; sampler and fitter are **component** evidence.

## What passed

The correction was inferred only from literal dense K129 predictions under
13 spherical-Poisson radii.  The R2 capture used 64 randomized orthogonal-
Latin replicates.  A separate process fit the frozen four-band model and
sealed its correction before a target was mapped.

On 16 disjoint confirmation networks:

```text
surface                 q0 raw MSE       corrected raw MSE   ratio
pooled                  2.538466156e-7   1.552563511e-7      0.611615
Full8                   1.932944411e-7   1.111348743e-7      0.574951
Generated8              3.143987901e-7   1.993778279e-7      0.634156
replicate half A        2.538466156e-7   1.482611101e-7      0.584058
replicate half B        2.538466156e-7   1.671564915e-7      0.658494
permutation null        2.538466156e-7   2.598130872e-7      1.023504
```

- pooled raw reduction: `38.8385%`;
- rows improved: `16/16`;
- independently fitted half-correction correlation: `0.727989`;
- real/null ratio margin: `0.411889`;
- row-bootstrap 95% ratio interval: `0.579068 .. 0.640128`;
- bootstrap probability of ratio `<=0.65`: `0.9965`.

Every preregistered gate passed.

## What was learned from R1

R1's eleven-column inversion was correctly killed.  It was numerically
ill-conditioned, and its initial exact-target sum constraint allowed a
permutation null to look helpful.  R2 removed that target leakage by fitting
`Q(P_rho f)-Q(f)` directly and sealing the resulting correction before target
access.  The R2 permutation null slightly worsened MSE, while the real
correction improved every row.  This is the controlling distinction.

## Scientific conclusion

The fixed-network signed response of K129 to its uncontrolled even angular
bands is real, broad, and recoverable.  This is stronger than the earlier
annealed NNGP energy audit and stronger than a multi-frame teacher: it produces
a target-free correction vector whose sign transfers across two network
families.

The result does **not** show that the correction is cheap.  R2 used 832
additional complete smoothed populations and is only an overpowered oracle.
It does not establish compatibility with R87's compressed numerical chassis,
an operation count, a wall receipt, Mini100 transfer, or a remote score.

## Next gate

Train or derive a compact weight/trajectory observable against the now-sealed
low-band correction, with whole-network cross-fitting.  Require:

- at least 70% retention of the oracle correction gain through actual dense
  ReLU transport;
- production-shaped incremental effective work no more than 15B;
- unchanged cross-family raw reduction at least 20%.

Only after those gates may an estimator implementation be considered.

## Receipts

- `runtime/artifacts/signed_poisson_sampler_validation_r1_component_20260806.json`
- `runtime/artifacts/signed_poisson_band_oracle_r2_broad_targetfree_20260806.json`
- `runtime/artifacts/signed_poisson_band_oracle_r2_correction_targetfree_20260806.json`
- `runtime/artifacts/signed_poisson_band_oracle_r2_broad_postseal_20260806.json`

No FlopScope estimator, package, upload, submission, or remote action was
created or authorized.
