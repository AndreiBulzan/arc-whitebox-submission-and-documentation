# Preregistration: signed spherical-Poisson band oracle R1

Date frozen: 2026-08-06

## Question and evidence boundary

This is an intentionally overpowered **component** oracle.  It asks whether
the signed, fixed-network K129 error carried by angular degrees `6..20` can
remove at least 35% of literal dense K129 raw MSE.  It is not a contest
estimator, a FlopScope graph, a package, a score projection, or evidence that
the bands can be transported cheaply.

The target-free capture reads only the named weight members.  Targets may be
opened only by the separate post-seal scorer after the capture, report, source,
and this preregistration are immutable and hashed.

## Prior-work preflight

Queries covered `Poisson semigroup`, `harmonic band`, `Gegenbauer`,
`harmonic_alias_decoder`, `degree6_support`, `closed_orbit_harmonic`,
`TensorSketch`, `ARC`, and `K-prop`.

- `degree6_support_mini100_20260806` showed that its apparent degree-six gain
  was only increased sample count.
- `k129_closed_orbit_harmonic_20260806` proved that many complete rotations
  contain cancellation, but did not recover a signed one-frame band.
- `harmonic_alias_decoder_20260729` killed fixed endpoint reweighting.
- `tensor_sketch_connected_k4_20260729` showed ordinary moment sketches are
  not closed through ReLU.
- the deterministic K129/NNGP audit already proves broad *annealed* band
  energy, not a signed correction for a supplied network.

Outcome: **materially new oracle observable**.  Spherical-Poisson smoothing
separates angular degrees by the exact multiplier `rho**k`; independently
split randomized captures then test whether the signed low-band correction is
stable.  No prior receipt measured this object.

## Frozen population and convention

- dimension `d=256`, depth `32`, bias-free ReLU;
- literal dense propagation `h <- relu(h @ W_l)` in fp32 with TF32 disabled;
- canonical K129 is reconstructed from
  `root_kerdock258_compact_exact_fwht_20260714.npz` orientation zero;
- its 33,024 unit lines are propagated with both signs;
- spherical outputs are multiplied by the exact Gaussian radial mean
  `sqrt(2) Gamma(128.5) / Gamma(128)`;
- no production compression, moment repair, output-scale fit, or target is
  present in capture.

Frozen rows:

```text
official Full1000: 600, 601, 602, 603
Generated128:      56,  57,  58,  59
```

Within each family, the first two rows are calibration and the last two are
held.  The pilot is not broad promotion evidence; it decides whether a larger
capture is warranted.

## Exact smoothing identity and sampler

For the spherical Poisson operator, a degree-k harmonic obeys

```text
P_rho f_k = rho**k f_k.
```

For every Kerdock line `u`, draw `t` with density proportional to

```text
(1-t*t)**((d-3)/2) / (1 + rho*rho - 2*rho*t)**(d/2),  -1 < t < 1,
```

draw `w` uniformly on the unit sphere in `u`'s orthogonal complement, and
form `v = t*u + sqrt(1-t*t)*w`.  Then `v` has the spherical Poisson kernel
centered at `u`.

One replicate uses a randomized shifted 256-point Latin grid for `t` and a
Haar orthogonal frame for the tangent directions, shared in a symmetry-
preserving way across all 129 bases.  Every individual line retains the exact
required marginal; dependence only reduces or increases variance and cannot
change the expectation.  Both `v` and `-v` are propagated.

Frozen Poisson radii:

```text
0.40, 0.50, 0.60, 0.68, 0.74, 0.79, 0.83,
0.87, 0.90, 0.93, 0.955, 0.975, 0.990
```

There are 16 independent replicates.  Replicates `0..7` and `8..15` are the
two sealed reliability halves.  Base seed: `2026080601`.

## Blocking sampler gate

Before network capture, the same numerical inverse CDF must pass:

1. finite samples strictly inside `(-1,1)`;
2. unit-norm perturbed nodes and exact antipodal pairing;
3. for every frozen radius and even degree `0..24`, the randomized-Latin mean
   of normalized Gegenbauer polynomials agrees with `rho**k` within the
   prespecified Monte-Carlo tolerance recorded by the validation source;
4. the validation receipt status is `pass`.

Failure blocks the network run.

## Frozen post-seal inversion

The scorer uses columns

```text
rho**6, rho**8, ..., rho**20,
NNGP-weighted rho**k for k=22..30,
NNGP-weighted rho**k for k=32..44,
NNGP-weighted rho**k for k=46..80.
```

NNGP weights are the positive degree contributions frozen in
`k129_nngp_harmonic_audit_r1_component_20260806.json`.

The exact `rho=1` K129 error is imposed as a sum constraint: the final tail
coefficient equals the exact total error minus all other fitted coefficients.
A single ridge strength is selected on calibration rows by leave-one-radius-
out curve prediction from the frozen grid

```text
0, 1e-10, 1e-9, 1e-8, 1e-7, 1e-6, 1e-5,
1e-4, 1e-3, 1e-2, 1e-1, 1, 10.
```

That strength and row weights are then unchanged on held rows.  Corrections
are fitted independently from the two replicate halves and in consensus.
A deterministic within-network output permutation is the null control.

## Decision gates

Advance to a broader signed-band capture only if, on held rows:

- the consensus degree-`6..20` correction reduces pooled raw MSE by at least
  35% (`ratio <= 0.65`);
- each family separately has ratio `<= 0.72`;
- each replicate half separately has pooled ratio `<= 0.80`;
- the two half-corrections have pooled Pearson correlation `>= 0.40`;
- the real consensus improvement exceeds the permutation-null improvement by
  at least 15 percentage points.

If the central ratio passes but half reliability fails, the result is
`inconclusive_sampling_noise`, not a kill and not a pass; a higher-replicate
R2 may be preregistered.  Otherwise failure kills the signed-band-headroom
spelling.  Passing still does **not** authorize Tangent ARC implementation:
the separate compact representation and dense-ReLU transport gates remain.

No archive, upload, submission, or remote action is authorized.
