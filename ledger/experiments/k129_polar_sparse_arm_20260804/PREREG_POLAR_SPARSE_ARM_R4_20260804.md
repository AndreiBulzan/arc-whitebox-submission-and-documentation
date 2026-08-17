# Preregistration: sparse polar second arm R4

Date: 2026-08-04

Evidence sequence: target-free **component** endpoint capture on the already
fixed Full16, Generated16, and official Mini100 rows; selection on Full16 and
Generated16 only; then one unchanged literal **broad statistical** Mini100
score.  No FlopScope row, package, upload, or submission is part of this
experiment.

## Prior-art boundary

The capsule was searched for partial/sparse polar frames, polar endpoint
coresets, right-frame sparse arms, quartic-balanced partial frames, and the
historical K141/K146 right-Gram descendants.  Sparse right-Gram arms and
multi-frame quartic balancing failed transfer.  No prior attempt thins the
newly successful output-shrunk `polar(q0 + right + d2)` arm.  This experiment
therefore changes the frame whose endpoint direction is compressed; it does
not reopen the failed right-Gram support unchanged.

## Fixed experiment

For each MLP, capture all 129 final per-basis endpoints of the complete polar
frame.  On the pooled Full16+Generated16 endpoint table only, greedily fit the
complete polar correction from fixed support sizes

```text
m in {32, 48, 64, 80, 96, 104}
```

with basis zero excluded so the result maps to the existing nonzero-O1
geometry.  Coefficients minimize reconstruction of the complete polar
direction; support fitting never reads a challenge target.  For each frozen
support, score only the fixed output shrink grid

```text
alpha in {0.25, 0.50, 0.75}.
```

Choose `(m, alpha)` by the smaller worst-family projected adjusted score on
Full16 and Generated16.  Ties choose lower `m`, then lower `alpha`.  Freeze
support, coefficients, alpha, and hashes before opening Mini100 targets.

The cost screen is deliberately conservative and is only a **projection**:

```text
C(m) = 141.1403492625B + 0.999B*m + 5B frame-construction allowance.
```

The selected rule is then rerun literally as an independently repaired sparse
arm, not scored from the full-frame endpoint linearization.  Only this literal
prediction is eligible for the Mini100 decision.

## Gates

Continue to current-meter physical pricing only if the literal Mini100 result
simultaneously has:

1. projected adjusted score `<= 1.10e-7` under the frozen cost model;
2. raw-MSE ratio to q0 `<= 0.65`;
3. at least `75/100` rows improved; and
4. paired-bootstrap raw-ratio upper 95% endpoint `< 0.80`.

Passing these gates does not validate runtime cost.  It only promotes one
Mini100-validated numerical rule to a physical current-meter row.

