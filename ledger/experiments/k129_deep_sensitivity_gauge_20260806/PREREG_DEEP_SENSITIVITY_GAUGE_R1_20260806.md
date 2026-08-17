# K129 deep-sensitivity ReLU gauge R1

Date: 2026-08-06

Evidence sought: target-free **component** predictions followed by a sealed
development diagnostic on fixed, selection-disjoint Full and Generated rows.
No FlopScope run, physical row, package, upload, submission, or remote action
is authorized.

## Prior-work boundary

The repository was searched for TAP/cavity, harmonic/geometric, gauge,
weight-aware selection, pullback selection, and compression experiments.
Relevant prior work closes:

- ordinary TAP/Onsager and one-site cavity corrections;
- global harmonic decoders, geodesic/polar interpolation, great-circle
  conditioning, Schur/quartic geometry, and naive multi-frame interlacing;
- late selection by activation second moment times the *immediate* outgoing
  row norm (`k129_weight_aware_selection_20260731`), which was effectively
  neutral;
- learned gauge-equivariant closures and downstream-sensitive input-frame
  selectors.

No prior artifact applies a whole-network positive diagonal ReLU gauge so
that every existing analytic and empirical compression screen sees a
full-depth sensitivity-weighted activation energy.

## Exact identity and fixed rule

For positive diagonal hidden gauges `D_l`, replace

```text
W_0  -> W_0 D_0
W_l  -> D_(l-1)^-1 W_l D_l       (1 <= l <= 30)
W_31 -> D_30^-1 W_31.
```

Positive homogeneity of ReLU makes the represented MLP exactly unchanged in
real arithmetic.

Starting from an all-ones final sensitivity, define a fixed diagonal
mean-field squared pullback using gate probability one half:

```text
v_(l-1) = 0.5 * (W_l ** 2) v_l,
mean(v_(l-1)) = 1,
D_(l-1) = diag(sqrt(v_(l-1))).
```

There is no exponent, clipping, fitted coefficient, or target-dependent
choice. After gauging, the incumbent energy screen ranks approximately
`E[h_j^2] * v_j`, while all exact network products remain unchanged.

## Fixed falsifier

- Full rows 20--31 and Generated rows 20--31;
- unchanged K129 O0 replay and incumbent output scale;
- variants: original control and the single deep-sensitivity gauge;
- verify the original and gauged float32 networks on eight deterministic
  Gaussian inputs per row;
- seal every prediction before opening targets.

Promote only if all conditions hold:

1. aggregate raw-MSE ratio is at most `0.985` on each family;
2. pooled ratio is at most `0.980`;
3. at least 7/12 rows improve in each family;
4. deterministic network-probe relative RMSE is at most `5e-5` on every row.

Otherwise kill this spelling without scanning exponents or clipping ranges.

