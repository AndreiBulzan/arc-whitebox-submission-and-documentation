# Layer-grouped gate-source L1/R1 preregistration

Date: 2026-08-08

Evidence scope: target-free full-population **component** capture and separate
post-seal capacity scoring.  This is an output-capacity oracle; production
costs are unresolved and no score forecast is licensed from capacity alone.

## Question

Scalar gate-source sampling is destroyed by cancellation, and complete-basis
sampling needs too many bases.  Does summing every centre and neuron within a
layer before sampling preserve enough cancellation to make the 32 exact
layer-source vectors a compact representation of the packet correction?

For canonical gates `D_l`, define the exact secant source

```text
b_l = relu(z_l^0 + delta_z_l) - relu(z_l^0) - D_l delta_z_l.
```

Transport `b_l` through the actual downstream canonical Jacobian and average
over all 66,048 antipodal centres.  This gives 32 vectors `Y_l` satisfying
`sum_l Y_l = Delta_source` after antithetic cancellation of the input tangent.

## Fixed capture

Use already-open Full rows 640,641 and Generated rows 88,89, all 129 bases,
all 256 lines per basis, and the sealed `epsilon=0.20` packet law.  Use 16
independent Gaussian-noise replicates with exact `+z,-z` antithetic pairs.
Store:

- layer-by-basis source atoms for all 32 layers;
- full, first-noise-half, and second-noise-half variants;
- a direct packet endpoint from the identical noises;
- the maximum exact source-sum identity discrepancy.

No targets are opened during capture.

## Capacity tests

Post-seal, report for `S=1,2,4,8,16`:

- uniform iid layer sampling;
- oracle norm-proportional iid layer sampling;
- uniform sampling without replacement;
- deterministic equal-weight per-row and universal output-oracle subsets.

Also report exact adjacent partitions into 2, 4, 8, and 16 contiguous layer
blocks.  These are representation diagnostics, not cost claims.

## Gates and careful stopping

Layer grouping survives the capacity gate if an `S<=4` sampler or
deterministic subset forecasts at least 35% raw reduction in both families,
the two noise halves retain positive reduction, and the source identity
closes within Monte Carlo/float32 uncertainty.

A survivor licenses a separate production-cost derivation only.  It does not
prove a `<=12B` implementation.

If every output-oracle layer construction through `S=8` fails to reach 35%
pooled reduction and the halves agree, then this exact layer partition is
rejected.  The result does not kill other correlated multi-layer blocks,
adaptive partitions, or non-source estimators.

## Integrity

- Hold `runtime/.benchmark_lane.lock` during GPU capture.
- Refuse overwrite and pin source, preregistration, compact asset, grouped
  packet capture, input archives, and output.
- No Mini100, FlopScope, physical runner, package, upload, or submission is
  authorized.
