# K139 / m10 operating-point R1 preregistration

Date: 2026-07-29

Evidence sought: a fast **component** gate, followed only on success by one
**broad statistical** capture.  This authorizes offline target-free CUDA
replay and scoring on already-used Full/Generated development rows.  It
authorizes no FlopScope session, physical row, package, network, upload,
submission, or remote action.

## Target-free support freeze

Keep the complete 129-basis orientation-0 arm.  Select exactly ten nonzero
orientation-1 bases from the pinned target-free 129-by-129 training Gram
using the frozen lower-K algorithm:

1. greedily grow the omitted set, minimizing its Gram quadratic form with
   deterministic basis-index tie breaking;
2. at omitted size 119, apply the same deterministic best single-exchange
   local search, capped at 500 accepted exchanges and using relative
   tolerance `1e-12`;
3. retain the complement as `S10`.

The existing m109, m33, and incumbent m17 outputs are exact algorithm
regressions only.  No alternative m, support, objective, family weighting,
or local-search spelling may be tried.  Freeze and hash `S10` before opening
any prediction target.

## Numerical candidate

Run the actual production-aligned K146 CUDA replay parameterized to
`O1_BASES=10`, `TOTAL_K=139`, and the frozen `S10`.  Preserve all widths,
repairs, layer ordering, late energy selection, lambda-zero gamma readout,
float32 propagation, and final arm semantics.  Persist `q0` and `q1`; the
only admitted final prediction is the literal support blend

```text
prediction = (129*q0 + 10*q1) / 139
```

There is no alpha fit or diagnostic coefficient grid.

## Fast gate

Freeze predictions, q0/q1, weights/seeds indices, sources, and hashes before
opening targets for exactly:

```text
Full       [0, 1, 2, 3]
Generated  [2, 4, 5, 6]
```

Compare with the frozen production K146 prediction on the identical rows.
Promote to exactly one Full100 + Generated64 capture only if, independently
in both families:

```text
pooled final raw MSE / incumbent K146 raw MSE <= 0.98
maximum per-row MSE ratio                         <= 1.60
all candidate arrays finite
```

and a conservative projected K139 count/effective-price calculation,
including the banked prediction-preserving K146 decoder/layout rewrite,
still gives a credible route to adjusted score `<=1.2e-7`.  The conservative
route check uses the worse family ratio; it does not select a blend or tune
the support.

If any condition fails, kill K139/m10 without broadening.

## Broad gate

If promoted, capture the already-fixed K146 Full100 and Generated64 rows
exactly once, target-free and under the shared benchmark lock, then score in
a separate process.  Report pooled ratios, row-ratio p95/max, raw MSE, and a
remote-anchor-calibrated adjusted projection.  Do not access the virgin
lower-K confirmation bank.

