# Preregistration: ideal integral-preserving smoothing oracle R1

Date frozen: 2026-08-07

## Question

Before implementing Gaussian-packet moment propagation, does an exact
integral-preserving smoother remove at least half of canonical K129's raw
quadrature MSE at a local radius that is plausibly compatible with packet
closure?

This is a component-capacity test.  It does not test a closure, production
cost, FlopScope implementation, package, adjusted score, or remote transfer.

## Prior-art preflight

The mechanism is not wholly new.  The sealed R2 spherical-Poisson capture was
already an exact integral-preserving smoothing oracle, but it was previously
used only to reconstruct signed harmonic bands.  The new claim is that the
smoothed function itself may be approximated by local Gaussian-packet moment
propagation.

Prior global or mid-depth Gaussian closures do not answer this question:

- global moment-matched Gaussianization was bias-floored;
- the 16-component full-covariance response mixture had large residual error;
- final-hidden Rao--Blackwell Gaussian closure failed;
- sparse and Latin rotated mixtures incurred excessive conditional-sampling
  noise.

The proposed packet state is materially different only because it keeps one
local component per Kerdock node and has a continuous zero-noise limit.  This
preregistration authorizes only the ideal-capacity gate, not that claim.

## Frozen inputs and evidence limitation

Use the unchanged target-free capture:

`runtime/artifacts/signed_poisson_band_oracle_r2_broad_targetfree_20260806.npz`

It contains 64 independently randomized smoothing replicates at 13 fixed
Poisson radii on disjoint Full8 and Generated8 rows.  The capture was sealed
before targets were opened.  However, these rows' targets have since been
opened by the prior R2 scorer.  This new direct-curve analysis is therefore a
**post-seal exploratory reanalysis**, not a new confirmatory split.  A passing
result must be confirmed later by the packet oracle on fresh rows.

## Frozen estimand

For prediction replicate `p_r` and target `y`, estimate the squared error of
the inaccessible exact conditional mean without finite-replicate bias:

```text
U = ((sum_r (p_r-y))^2 - sum_r (p_r-y)^2) / (R (R-1)).
```

Average `U` over output coordinates and networks.  Report the ordinary
64-replicate plug-in MSE, its inferred conditional-Monte-Carlo contribution,
and the same U-statistic separately for replicates 0--31 and 32--63.

The primary radius is frozen as `rho=0.975`.  Under the low-degree relation
`rho ~= exp(-epsilon^2/2)`, this corresponds to `epsilon ~= 0.225`.  It was
chosen from deterministic NNGP arithmetic, not these network targets.  All
other radii are diagnostic only; no best-radius result is a gate.

## Independent harmonic calculation

Using the frozen depth-32 K129 NNGP audit through degree 80, compute for shell
smoothing

```text
sum_k contribution_k * P_k(cos(epsilon))^2,
```

and report both the resolved sum and a conservative upper bound obtained by
leaving the entire unresolved degree-82+ tail undamped.  This is deterministic
component arithmetic and is not evidence about a fixed finite network.

For Poisson smoothing, use multipliers `rho^k`.  Since all NNGP band-energy
contributions are nonnegative, bound the unresolved tail at the primary radius
by multiplying it by `rho^(2*82)`.

## Frozen gates

All are required for `pass_ideal_smoothing_capacity`:

- primary pooled unbiased MSE ratio to q0 `<= 0.50`;
- primary Full ratio `<= 0.65`;
- primary Generated ratio `<= 0.65`;
- each 32-replicate-half pooled ratio `<= 0.70`;
- at least 12 of 16 row U-statistics are below the corresponding q0 MSE;
- 20,000-row-bootstrap probability that the pooled ratio is `<=0.50` is at
  least `0.90`;
- the deterministic degree-80-plus-tail NNGP upper-bound ratio at `rho=0.975`
  is `<=0.50`.

A pass authorizes only Stage B: high-sample measurement of the true
Gaussian-packet-smoothed estimator on fresh networks.  It does not authorize
packet closure, estimator changes, packaging, upload, score projection, or
submission.

