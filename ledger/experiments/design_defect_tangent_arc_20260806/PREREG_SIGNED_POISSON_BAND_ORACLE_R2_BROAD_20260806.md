# Preregistration: target-free signed Poisson-band oracle R2 broad

Date frozen: 2026-08-06

## Why R2 exists

R1's eleven-column, target-error-constrained inversion was killed: nearly
collinear columns amplified smoothing noise by orders of magnitude.  A
permutation null then exposed a more fundamental problem with that formulation:
using the exact target error as the coefficient-sum constraint permits a
trivial shrinkage of the answer even when the smoothing information is
destroyed.

R2 removes that leakage.  It infers the constant mode and signed correction
from target-free predictions alone.  R1 rows and targets were used only as a
burned selection bank to freeze one low-dimensional spelling.  No R2 row,
prediction, or target has been captured or inspected at freeze time.

## Frozen spelling

The target-free observations obey

```text
Q(P_rho f) = mu + sum_g c_g b_g(rho),
Q(f)       = mu + sum_g c_g.
```

Eliminating the unknown constant mode gives

```text
Q(P_rho f) - Q(f) = sum_g c_g (b_g(rho) - 1).
```

The four frozen band shapes are:

1. NNGP-weighted degrees `6..20` (the desired correction);
2. NNGP-weighted degrees `22..40`;
3. NNGP-weighted degrees `42..80`;
4. one conservative unresolved-tail proxy `rho**120`.

NNGP weights are the deterministic positive contributions in the unchanged
degree-80 harmonic audit.  Ridge is fixed at `1.0` after column
normalisation.  There is no coefficient search, scale fit, target-derived
constraint, or alternate spelling in R2.

The K129 reconstruction, Poisson radii, fp32 dense network propagation,
fp64 terminal reduction, exact Gaussian radial multiplier, and orthogonal-
Latin coupling are identical to R1 and inherit the passed sampler receipt.

## Frozen rows, randomness, and evidence split

```text
official Full1000: 620,621,622,623,624,625,626,627
Generated128:      72,73,74,75,76,77,78,79
Poisson radii:      0.40,0.50,0.60,0.68,0.74,0.79,0.83,
                    0.87,0.90,0.93,0.955,0.975,0.990
replicates:         64
replicate halves:   0..31 and 32..63
base seed:          2026080621
```

All 16 networks are confirmation rows.  R1 selected the spelling; R2 may not
select another grouping, ridge, radius, row weight, or scale.

## Three-process seal

1. The capture reads weight members only and seals all q0 and smoothed
   predictions.
2. A target-free fitter computes and seals the all-replicate, half-A, half-B,
   and permutation-null corrections.  Per-radius weights may depend only on
   replicate variance in the R2 capture.
3. Only the post-seal scorer may open R2 targets.

The scorer must reject any hash drift between these stages.

## Gates

Target-free fitter gate:

- finite corrections;
- Pearson correlation between half-A and half-B low-band corrections at
  least `0.40`.

Post-seal capacity gate, all required:

- pooled corrected/q0 raw ratio `<= 0.65`;
- Full ratio `<= 0.72`;
- Generated ratio `<= 0.72`;
- each replicate-half ratio `<= 0.80`;
- permutation-null ratio `>= 0.90`;
- real/null ratio margin at least `0.20`;
- at least 12 of 16 rows improve;
- row-bootstrap probability of pooled ratio `<=0.65` at least `0.90`
  over 20,000 fixed-seed resamples.

Passing establishes broad signed-band **oracle capacity** only.  It authorizes
the next compact weight/trajectory representation experiment, not a contest
estimator, FlopScope implementation, package, upload, score claim, or remote
submission.
