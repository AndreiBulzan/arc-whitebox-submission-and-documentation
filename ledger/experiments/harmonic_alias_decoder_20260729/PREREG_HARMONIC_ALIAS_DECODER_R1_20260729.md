# Harmonic alias decoder R1 preregistration

Date: 2026-07-29

Evidence sought: **component**. This is an existing-atlas signed-endpoint
falsifier. It authorizes no estimator/GPU/FlopScope run, physical benchmark,
package, upload, submission, or remote action.

## One fixed mechanism

The paid K146 rule exposes all 129 orientation-0 per-basis signed endpoints
and the frozen 17 orientation-1 endpoints. The complete eight-orientation
population is a much lower-alias target-free reference. R1 asks whether the
omitted degree-6-and-higher alias is linearly decodable from those 146 paid
basis values.

For each network/output coordinate, form

```text
x       = [O0 all129, O1 frozen-S17]             shape (146,)
b       = mean(x)
d       = x - b
t_proxy = mean(all 8 * 129 basis endpoints)
```

Fit one fixed, output-shared correction

```text
candidate = b + d @ c
```

by ridge regression to `t_proxy - b`. Since every row of `d` sums to zero,
the effective quadrature weights sum exactly to one. No endpoint target,
network identity, output identity, weight feature, family flag, nonlinear
model, or row-specific coefficient is used.

The ridge grid is fixed:

```text
0, 1e-4, 1e-3, 1e-2, 1e-1, 1, 10, 100
```

Ridge is scaled by the mean Gram diagonal and selected only by a
deterministic 50/50 split inside the source family's frozen training half.
The coefficient is then refit on that complete source-training half.

## Strict reciprocal test

Run both directions without sharing fitted coefficients:

```text
Full frozen train100      -> Generated frozen held64
Generated frozen train64 -> Full frozen held100
```

The target-free capture serializes and hashes both held predictions before
the separate scorer opens either exact signed-preactivation target.

## Hard gate

Promote only if both reciprocal held families independently satisfy:

```text
candidate / literal-K146 signed MSE <= 0.80
row-ratio p95                      <= 1.25
all predictions finite
```

Failure closes this fixed linear alias-decoder spelling. It also closes a
fixed nonuniform weighting of the existing paid K146 basis endpoints,
because every such weighting is represented by `b + d @ c`.

## Economics

At deployment the fitted rule is only one fixed weighted reduction of the
already-computed 146 by 256 per-basis endpoint table. Its ordinary arithmetic
is below `0.001B` operations. This is a **projection**, not a FlopScope
receipt.
