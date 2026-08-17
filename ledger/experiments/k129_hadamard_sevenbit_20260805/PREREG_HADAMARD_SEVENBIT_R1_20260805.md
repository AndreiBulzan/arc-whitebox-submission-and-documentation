# Hadamard-incoherent seven-bit contraction R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical capacity and static
cost **projection** only. No scoring, FlopScope execution, physical row,
package, upload, submission, or remote action.

## Prior-work boundary and exact mechanism

Capsule searches for Hadamard/incoherence/QuaRot/QuIP quantization found no
prior attempt. SmoothQuant-style diagonal balancing and activation-outlier
splitting both improved endpoint discrepancy but failed the fixed nested
correction-variance gate. Plain seven-bit blocked packing was close but missed
Generated (`0.10896` versus `0.08`).

QuIP#/QuaRot motivate the exact product-basis change

```text
L W = (L Q) (Q^T W),       Q orthogonal,
```

before scalar quantization. This does not rotate a hidden ReLU state; it is a
strictly local factorization of one linear product, so the unquantized network
is unchanged.

R1 fixes `Q` without target selection. For every contracted width, apply one
deterministic pseudorandom permutation and Rademacher sign vector (seed
`20260805 + width`), then independent normalized Walsh-Hadamard transforms in
blocks of eight. All active widths in layers 2--23 are divisible by eight.
No rotation, sign, block-size, or seed sweep is permitted.

## Fixed fast falsifier

- Full row 0 and Generated row 2;
- sealed K64/K16 calibrated-cloud emulator;
- quantize exactly layers `2..23`;
- unsigned seven-bit affine activation and weight codes;
- all clipping/calibration/nesting/readout settings unchanged;
- same-support dense float32 cloud as control;
- verify the unquantized transformed product before scoring the quantized
  product;
- no target access and no FlopScope execution.

Promote only if both families have residual variance ratio at most `0.08`,
multilevel discrepancy MSE below `5e-7`, and transformed-product relative MSE
below `1e-12`. All permutation, gather, sign, Hadamard, quantization,
extraction, and movement operations must be priced before deployment.

