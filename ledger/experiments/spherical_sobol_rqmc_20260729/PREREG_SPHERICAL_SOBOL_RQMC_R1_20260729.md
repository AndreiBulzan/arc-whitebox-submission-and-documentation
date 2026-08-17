# Spherical Sobol RQMC R1 preregistration

Date: 2026-07-29

Evidence scope: **component**. This is an ordinary-CUDA, target-free
quadrature-design falsifier. It is not a FlopScope estimator, a measured
whole, a package, or a remote result.

## Question

At the same number of antipodal spherical nodes, does a fixed Owen-scrambled
Sobol digital net reduce depth-32 final-mean error enough to justify replacing
the Kerdock angular cloud?

The comparison deliberately isolates the angular rule:

- `K=16`, hence `4,096` positive spherical nodes and their antipodes;
- fixed-radius sphere propagation with the exact analytic Gaussian radial
  mean restored at readout;
- the same exact first-layer marginal mean/variance repair for both rules;
- otherwise literal dense float32 ReLU propagation through all 32 layers;
- Kerdock control: eight bases from each of the two production orientations;
- Sobol candidate: `2^12` points in 256 dimensions, Owen scrambling with
  fixed seed `2026072901`, inverse-normal map, then row normalization.

The Sobol seed is a fixed estimator constant. It is not selected per network
and no target is used in construction.

## Frozen rows and gate

Use the already-open component rows:

- Full `{0, 1, 100, 101}`;
- Generated `{0, 1, 64, 65}`.

The capture must write both prediction banks before a separate scorer opens
targets. Continue this class only if:

```text
Full pooled raw ratio Sobol / Kerdock                  <= 0.80
Generated noise-corrected pooled ratio                 <= 0.80
each fixed two-row half ratio in both families         <= 1.00
all outputs finite
```

This is intentionally a strong gate. The production Kerdock primary arm is
a near-minimal exact spherical 5-design and the deep ReLU integrand is
high-dimensional and non-smooth. A small or one-family Sobol win would not
overcome the loss of exact low-degree cancellation, the need to port all
current repairs, or implementation risk.

No physical run, package, upload, or remote action is authorized.
