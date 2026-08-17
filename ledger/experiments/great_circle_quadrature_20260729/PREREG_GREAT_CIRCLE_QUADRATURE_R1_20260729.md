# Great-circle quadrature R1 preregistration

Date: 2026-07-29

Evidence scope: **component**. This is an offline, target-free support-design
falsifier. It does not run FlopScope, build a package, upload, or submit.

## Question

Can conditional angular integration inside Haar-distributed two-dimensional
subspaces reduce the final-layer quadrature error at fixed propagated support?

For a standard Gaussian input, write `x = r u`, with `u` uniform on the unit
sphere and `r` independent. A bias-free ReLU MLP is positively homogeneous,
so its Gaussian mean is `E[r] E[f(u)]`. If a two-plane is Haar distributed,
a uniformly random angle in that plane is marginally uniform on the sphere.
Replacing random angles by a uniform polygon is therefore a lawful
Rao--Blackwell-style angular quadrature, not an accounting device.

## Fixed pilot

- geometry: depth 32, width 256;
- Full rows: `3,103,203,303`;
- Generated rows: `3,35,67,99`;
- propagated directions per method: exactly `4096`;
- arithmetic: float32 propagation, float64 scoring;
- common layer-1 exact radial first/second-moment repair;
- radial output scale:
  `E[chi_256] = sqrt(2) Gamma(257/2) / Gamma(256/2)`;
- support seed: `2026072901`;
- targets remain unopened until the capture NPZ and its source hash exist.

## Methods

1. `kerdock_k8`: the existing two-orientation Kerdock support with eight
   complete bases and both signs (`8 * 256 * 2 = 4096` directions).
2. `haar_circle_q4`, `q8`, `q16`, `q32`: adjacent coordinate pairs from
   respectively `8,4,2,1` independent Haar orthonormal bases, with a uniform
   `q`-gon on each resulting great circle. Every support has exactly 4096
   directions and exact empirical covariance `I/256`.
3. `kerdock_circle_q8`, `q16`, `q32`: the same angular polygons built from
   adjacent axes of `4,2,1` production Kerdock bases. These isolate angular
   integration from the underlying frame family. The omitted `q4` version is
   exactly the Kerdock control up to row ordering.

The same frozen supports are used on every MLP. No method sees weights while
constructing its support.

## Gates

Score the final-layer MSE separately on Full and Generated.

- **Breakthrough-promising:** one non-control method has pooled MSE ratio
  `<= 0.70` on both families relative to `kerdock_k8`, with neither family's
  worst-row median ratio above `1.0`.
- **Continue-worthy:** one method has pooled ratio `<= 0.90` on both families
  and no ratio above `1.05`.
- **Kill:** every method has a pooled ratio above `0.95` on either family, or
  a gain reverses materially between families.

A 4+4 pilot is not broad statistical evidence. Passing only authorizes a
larger target-free capture and a separately frozen score; it does not
authorize estimator integration or a physical run.

## Feasibility arithmetic if the gate passes

At equal support size the dominant cloud matrix products have the same dense
operation count as the Kerdock control. Polygon construction is setup-only
constant generation. A deployment design at K=146 therefore remains in the
same first-order cloud budget as the banked remote estimator, subject to
ordinary implementation, result-cap, residual, wall, and package gates.

