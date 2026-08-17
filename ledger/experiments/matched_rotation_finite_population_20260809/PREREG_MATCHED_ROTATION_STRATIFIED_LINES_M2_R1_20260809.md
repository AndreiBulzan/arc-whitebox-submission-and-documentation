# Matched-rotation stratified-line correction M2 R1

Date: 2026-08-09

## Reopening condition

M1 proved that uniformly sampling complete rotated bases fails because the
between-basis variance of the matched frame difference is too large.  M2
changes the sampling unit.  It samples a few signed rows or antipodal lines
inside **every** one of the 129 bases, so the between-basis component is
integrated exactly rather than sampled.

Capsule searches covered `paired polar`, `matched basis`, `endpoint MLMC`,
`stratified`, `antipodal line`, `one row per basis`, `transversal`, and the
polar/q0 final-readout hooks.  The nearest controlled works sampled complete
bases or transformed final preactivations across bases.  None computed the
within-basis finite-population risk of matched q0/polar node differences.
This is a **materially new sampling design**.

## Exact statistic

For basis `b`, line `h`, sign `s`, let `d[b,h,s]` be the polar-minus-q0
repaired final-ReLU output contribution.  The complete frame difference is

```text
d_bar = mean_{b,h,s} d[b,h,s].
```

Test three exact without-replacement designs:

1. `paired`: sample `l` line indices per basis and average their antipodal
   pair contributions;
2. `independent_sign`: sample `l` positive and `l` negative line indices per
   basis independently;
3. `signed`: sample `m` of the 512 signed rows per basis.

For each design the capture stores the full mean difference and the exact
within-basis population covariance trace.  The post-seal expected MSE of

```text
q0 + alpha * estimated(d_bar)
```

is then an exact scalar quadratic, not a Monte Carlo estimate.

## Frozen scope

- Frame: `polar_q0_right_d2`, unchanged from the sealed broad polar capture.
- Rows: the existing Full100 and Generated128 row manifests.
- Capture: ordinary CUDA only, target-free, under
  `runtime/.benchmark_lane.lock`.
- No FlopScope, package, physical runner, upload, submission, or remote work.
- Pair grid: `l in {1,2,4,8,12,16,24,32,48,64,96,128,192,256}`.
- Signed grid: the corresponding `m=2l`, capped at 512.
- One scalar `alpha in [0,1]` is selected jointly over the two broad families
  and applied unchanged to each.

## Conservative economics

- R94 adjusted reference: `1.1146386484507326e-7`.
- R94 effective reference: `137.8297337635B`.
- Frame fixed allowance: `5.0B`.
- Marginal complete-basis allowance: `0.999B`.
- `l` antipodal samples in every basis cost the row-equivalent of
  `129*l/256` complete bases.

## Promotion gate

Promote only if one design, with at most 32 complete-basis-equivalents:

1. projects at or below `8.0e-8` on Full and Generated separately;
2. has expected raw ratio at most `0.70` on both;
3. improves at least 75% of rows in each family in expected conditional MSE;
4. associates both complete q0 and polar predictions within `5e-5` maximum,
   with the internally reconstructed means within `2e-10`.

Failure closes uniform within-basis stratification for this polar frame.  It
does not close a different frame, adaptive line probabilities based on
already-paid q0 observables, or a deterministic cubature over line indices.

