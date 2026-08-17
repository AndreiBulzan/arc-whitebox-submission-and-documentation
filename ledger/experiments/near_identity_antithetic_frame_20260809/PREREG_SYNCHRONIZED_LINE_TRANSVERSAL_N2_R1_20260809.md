# Preregistration: synchronized antithetic line transversals N2/R1

Date: 2026-08-09

Evidence scope: target-free ordinary-CUDA component capture followed by a
post-seal 16-Full/16-Generated expected-risk score.  No FlopScope session,
physical row, package, Mini100 run, upload, submission, or remote action is
authorized.

## Motivation and boundary

N1 established that complete opposite weight-eigenplane frames have strong
cross-family capacity, but independent line sampling loses essentially all of
it.  N1's variance calculation sums the within-basis line variances and hence
does not retain covariance cancellation between bases.

N2 samples one *transversal index*.  For a frozen permutation `p_b` in each
basis, transversal `r` contains line `p_b(r)` from every one of 129 bases.
The vector contribution is averaged across bases before its finite-population
variance over the 256 transversal indices is computed.  Sampling `l`
transversals therefore preserves any cross-basis cancellation induced by the
common algebraic index.

This is not a new neural trajectory, selector, target model, or independent
per-basis line sample.  It is a different lawful finite-population unit built
from the same opposite-frame rows.

## Frozen angles and transversal maps

Retain all six N1 angles:

```text
pi * (1/64, 1/32, 1/16, 1/8, 1/4, 1/2).
```

The following maps are frozen before target access:

1. `identity`: `p_b(r)=r`;
2. `xor_basis`: `p_b(r)=r xor b`;
3. `add_basis`: `p_b(r)=(r+b) mod 256`;
4. `xor_bitreverse_basis`: `p_b(r)=r xor bitreverse8(b)`;
5. `random_permutation_0`: independent permutations from seed `2026080901`;
6. `random_permutation_1`: independent permutations from seed `2026080902`.

The random maps are frozen design randomization, independent of weights,
outputs, network IDs, and targets.

## Exact risk and economics

For each row, angle and map, seal the complete correction `d` and the trace
of the 256 transversal vectors about `d`.  For uniform sampling without
replacement,

```text
v_l = (1-l/256)/l * trace_transversal.
```

One sampled transversal evaluates `l` antipodal lines in every basis for
both opposite rotations, so its trajectory cost remains

```text
2 * 129 * l / 256 complete-basis equivalents.
```

Use the same conservative projection as N1:

```text
extra effective B = 5 + 0.999 * equivalents.
```

After the target-free artifact is sealed, optimize one shared positive scalar
in `[0,1]` jointly over Full16 and Generated16 for each frozen
angle/map/count.  This is a necessary-condition component oracle, not a
production selection rule.

## Gate

Promote exactly one fixed angle/map/count to a broad target-free capture only
if it:

1. uses no more than 40 complete-basis equivalents;
2. has expected raw ratio at most `0.70` separately on Full and Generated;
3. projects to at most `9.0e-8` separately on both families; and
4. improves at least 11/16 expected rows in each family.

If several pass, choose the lowest worst-family projected adjusted score,
then the lowest equivalent count, then the order above.  The broad capture
must use the chosen rule unchanged and must pass `8.5e-8` before any exact
Mini100 implementation.

Failure closes these six synchronized mappings for the adjacent-eigenplane
generator.  It does not close an optimized deterministic cubature over line
indices.
