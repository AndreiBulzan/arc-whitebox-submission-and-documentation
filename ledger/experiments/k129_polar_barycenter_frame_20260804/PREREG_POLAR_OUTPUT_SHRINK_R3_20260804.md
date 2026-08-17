# Preregistration: polar-output shrink R3

Date: 2026-08-04

## Question

Can a fixed shrinkage of the already selected `polar_q0_right_d2` correction
toward the ordinary q0 prediction retain its mean gain while suppressing the
network-to-network tail that made polar R1 fail its transfer gate?

This differs from R2: R2 anchored the frame *before* its polar factor and
destroyed the useful cancellation.  R3 leaves both complete-frame predictions
unchanged and blends only their final outputs:

```text
y(a) = y_q0 + a (y_polar - y_q0).
```

The capsule was searched for `polar.*blend`, `blend.*polar`,
`polar.*shrink`, and `shrink.*polar`; no prior output-space polar correction
test was found.  Generic scalar output calibration is not the same operation.

## Frozen rule

- Source predictions: the sealed polar R1 capture.
- Correction direction: `polar_q0_right_d2 - q0`, fixed by R1 before this
  experiment.
- Candidate coefficients: exactly `{0.25, 0.50, 0.75, 1.00}`.
- Select the coefficient minimizing the worse raw-MSE ratio on the existing
  Full16 and Generated16 pilot populations; ties choose the smaller value.
- Apply the selected coefficient unchanged to all official Mini100 rows.
- No coefficient is fitted or selected on Mini100.

## Gates

Promote to physical pricing only if Mini100 simultaneously gives:

1. raw-MSE ratio to q0 `<= 0.92`;
2. at least `60/100` rows improved; and
3. paired-bootstrap 95% ratio upper endpoint `< 1.0`.

Otherwise kill output-space polar shrinkage without refining the grid on
Mini100.  Any adjusted score remains a projection until its full construction
is priced through the current physical client.

