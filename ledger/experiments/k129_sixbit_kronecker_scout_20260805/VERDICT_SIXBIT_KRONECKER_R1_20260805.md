# Six-bit Kronecker/SWAR R1 verdict

Date: 2026-08-05

Evidence label: target-free **component**.

## Result

The exact packing construction is valid, but the numerical estimator lane is
killed.

- Three unsigned six-bit products fit exactly in one uint64 contraction lane.
- The static dense-contraction tariff falls from 511 to 342 per scalar output
  before transforms (`0.669276x`).
- On Full row 0, the best residual-variance ratio was `0.142851`.
- On Generated row 2, the best residual-variance ratio was `0.263706`.
- The preregistered gate required at most `0.08` on both families; Generated
  also missed the `5e-7` multilevel-discrepancy gate.

Artifact:
`runtime/artifacts/k129_sixbit_kronecker_r1_targetfree_20260805.json`.

## Interpretation

This closes the untested point between the old four-bit numerical failure and
the eight-bit cost break-even. Uniform six-bit affine propagation destroys too
much coupled signal for the exact packing discount to repay. Do not build the
FlopScope kernel or broaden this spelling. A future reopening requires a
strictly different mechanism, such as exact high-precision residual channels
selected before targets are opened; merely changing clips or calibration is
not enough.

