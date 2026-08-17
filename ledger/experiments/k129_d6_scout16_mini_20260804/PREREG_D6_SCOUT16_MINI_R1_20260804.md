# Preregistration: fixed D6 scout-16 pair on official Mini100

Date: 2026-08-04

Evidence sought: target-free **component** capture followed by one
**broad statistical** score on the independent official Mini100.  This is
an accuracy falsifier, not a FlopScope receipt, package, or remote action.

## Prior-art boundary

The rule already exists, prediction-sealed, in
`k129_fourframe_h2_mlmc_20260802/sparse_d6_scout_r4_targetfree_20260802.npz`.
It was candidate index 25: sixteen right-frame D6 basis proxies, sixteen
Jacobian-depth-2-frame D6 basis proxies, and ridge `0.01`.  All of its
supports, weights, coefficients, and Full/Generated predictions were sealed
before those development targets were opened.  Its post-seal Full100 and
Generated128 raw ratios were `0.8194339044` and `0.8242181426` at a nominal
support-layer cost ratio of `1.0480120030`.

This run does not refit that rule.  Full/Generated are now the selection
populations and official Mini100 is the untouched transfer population.  No
support, weight, coefficient, frame, depth, or output scale may change after
Mini100 is scored.

## Fixed estimator

Let `q0` be the complete K129 prediction.  At depth six, let `p0` be the
equal-basis q0 diagonal-continuation proxy.  Let `pr` and `pd2` be the frozen
weighted 16-basis proxies for the right and Jacobian-depth-2 frames.  The
candidate is exactly

```text
q0 + 5.274015078585109 * (pr  - p0)
   + 3.2969328795214095 * (pd2 - p0).
```

## Blocking gates

The rule advances to a capsule-native FlopScope implementation only if all
100 Mini rows are finite and:

```text
raw MSE ratio to matched q0                 <= 0.88
rows improved                              >= 60 / 100
paired-bootstrap ratio upper 95%            < 1.00
```

`0.88` deliberately leaves room for the sidecar's unmeasured physical cost.
After an accuracy pass, the deployment gate remains adjusted projection
`<=1.10e-7` using the R31 remote anchor and a measured whole FlopScope 0.10
receipt; the nominal `1.048` cost model is not sufficient for promotion.

