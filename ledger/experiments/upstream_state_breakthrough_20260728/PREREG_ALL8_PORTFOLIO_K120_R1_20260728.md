# Preregistration: all-eight minimum-variance basis portfolio, K120 r1

Date: 2026-07-28

Evidence target: **component**.  Any count/score arithmetic is a
**projection**.  This experiment cannot produce a measured whole, package,
upload, submission, or remote action.

## New observable and hypothesis

The production K146 rule spends 129 complete bases on orientation 0 and 17
on orientation 1.  That asymmetry is unusually good, but it leaves six
already-defined Kerdock orientations unused.  Earlier positive recombination
experiments selected K32/K64 atoms from shallow moment/response features, or
optimized each orientation separately.  This r1 instead asks a different,
strictly target-free question:

> Does the cross-orientation covariance of the *actual signed final
> preactivation basis endpoints* admit one fixed, stable K120 portfolio that
> approximates the complete eight-orientation population better than K146?

The final signed endpoint is the dominant diagnosed error channel.  The
endpoint atlas is a weights-only deterministic propagation artifact; its
complete eight-orientation mean is therefore an inference-admissible proxy,
not a challenge target.

## Immutable construction

Inputs:

- pinned Full200 and Generated128 eight-orientation endpoint atlases;
- only `indices.npy` and `final_basis_premean.npy` during capture;
- the already-frozen lower-K train/held split;
- fixed K146 support17
  `(6,9,11,37,41,42,48,52,55,58,64,65,79,108,111,112,128)`.

For each family training half, flatten `(MLP, output-neuron)` into samples.
For each of the 1,032 orientation/basis atoms, subtract the equal-weight
all-eight population endpoint on that sample and form its covariance Gram.
Normalize each family Gram by its own sample count, then average the two
family Grams with literal weight `1/2`.

Select exactly 120 atoms by deterministic forward minimum-variance
selection.  At every step, add the atom giving the largest increase in
`1' G_S^{-1} 1`, using a Schur-complement update.  No ridge is used.  After
120 steps, freeze

```text
w = G_S^{-1} 1 / (1' G_S^{-1} 1).
```

There is no target fit, MLP-specific routing, coefficient scan, endpoint
lambda, checkpoint, or retry.  Ties use the first flattened atom ID.  Atom
ID is `129 * orientation + basis`.

## One-shot gates

The capture freezes held candidate endpoints before any held exact signed
preactivation label is opened.

Structural/stability gates:

1. exactly 120 unique atoms spanning at least six orientations;
2. `abs(sum(w)-1) <= 1e-10`;
3. every weight nonnegative to numerical tolerance `-1e-12`;
4. `max(abs(w)) <= 0.02` and effective support
   `1/sum(w^2) >= 100`.

Target-free held compression gates, separately on Full100 and Generated64:

5. candidate MSE to the all-eight endpoint is at most `0.85` times the
   fixed K146 MSE to the same endpoint;
6. at least 55% of MLP rows improve versus K146;
7. rowwise compression-MSE ratio p95 is at most `1.25`.

Exact signed-final-mean gates, opened only by the post-seal scorer and
separately on both families:

8. aggregate signed endpoint MSE is at most `4.0e-7`;
9. aggregate signed endpoint MSE is at most `0.90` times fixed K146;
10. at least 55% of rows improve and rowwise ratio p95 is at most `1.25`.

All ten gates must pass.  Failure kills this exact K120 portfolio and forbids
changing K, ridge, family weight, support, or coefficient on these held
rows.

## Economics and promotion boundary

If work scaled exactly with propagated bases, K120 would use `120/146 =
0.821918` of K146 cloud work, an 17.81% reduction.  Applying that factor to
remote 320262's `171.0B` effective work gives `140.55B`; this is only a
projection because eight-orientation initialization, layout, repair, and
residual have not been implemented or metered.

At `140.55B`, a `1.2e-7` adjusted checkpoint permits raw MSE
`2.322e-7`; `1.0e-7` permits `1.935e-7`.  Passing the signed-endpoint gates
does not itself establish either final-ReLU raw value.  It authorizes one
small, disjoint two-family full-propagation scout with this exact sealed
support and weights.  Only that scout can justify a broad final-ReLU freeze
or a FlopScope implementation.

