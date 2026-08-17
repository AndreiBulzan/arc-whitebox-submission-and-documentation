# Stochastically rounded seven-bit contraction R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical capacity only. No target
score, FlopScope execution, physical row, package, upload, submission, or
remote action.

## Prior-work boundary and mechanism

Capsule search found no stochastic/unbiased/dithered quantization attempt.
Plain round-to-nearest seven-bit blocked packing was close to the fixed MLMC
gate; diagonal equalization, activation-outlier splitting, and an H8
incoherence transform all failed that gate.

R1 replaces round-to-nearest by ordinary stochastic rounding of the affine
activation and weight codes:

```text
q(y) = floor(y) + Bernoulli(y - floor(y)).
```

Before clipping, `E[q(y) | y] = y`. The hypothesis is that removing coherent
rounding bias lets the 64-base particle average absorb the resulting
zero-mean error. This is a numerical capacity screen only: a lawful stateless
pseudorandom spelling and all random-generation operations would need pricing.

## Fixed fast falsifier

- Full row 0 and Generated row 2;
- sealed K64/K16 calibrated-cloud emulator;
- quantize exactly layers `2..23`;
- unsigned seven-bit affine codes for both operands;
- fixed per-call seeds `20260805 + 2*call` and `+1`;
- reset the call counter once per network;
- no seed, bit-width, clipping, or window sweep;
- no target access and no FlopScope execution.

Promote only if both families have residual variance ratio at most `0.08` and
multilevel discrepancy MSE below `5e-7`. Otherwise kill this spelling.

