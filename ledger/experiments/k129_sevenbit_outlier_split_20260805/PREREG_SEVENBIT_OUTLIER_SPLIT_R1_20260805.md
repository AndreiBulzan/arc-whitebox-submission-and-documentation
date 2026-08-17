# Seven-bit activation-outlier split R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical capacity and static core
cost **projection** only. No target score, FlopScope execution, physical row,
package, upload, submission, or remote action.

## Prior-work boundary and mechanism

Capsule search found no outlier-separated quantized contraction. Plain
seven-bit blocked packing passed both final-discrepancy gates but missed the
Generated nested-residual gate (`0.10896` versus `0.08`). SmoothQuant-style
equalization reduced endpoint discrepancy but destroyed nested correction
correlation and is closed.

LLM.int8 and later outlier-separated GEMM methods use the exact decomposition

```text
L W = L[:, S] W[S, :] + L[:, T] W[T, :]
```

and keep activation-outlier channels `S` in full precision while quantizing
the complement `T`. This R1 uses no learned or target-fitted rule: for each
product, `S` is exactly the largest one eighth of contracted channels by
`max_i abs(L[i,j])`; the count is rounded only to preserve four-channel
blocking. The first term is dense float32 and the second uses the already
verified unsigned seven-bit blocked contraction.

## Fixed fast falsifier

- Full row 0 and Generated row 2;
- sealed K64/K16 calibrated-cloud emulator;
- fixed layers `2..23` (`eligible_first24`), with no window sweep;
- fixed 12.5% exact activation-outlier fraction;
- clipping, affine calibration, nesting, and readout unchanged;
- same-support dense float32 cloud as control;
- no target access and no FlopScope execution.

At width 256, the core projection is approximately

```text
32-channel dense branch        63
224-channel blocked branch    335
branch addition                 1
total                          399   versus dense 511 = 0.7808x
```

All selection, gather, quantization, correction, and movement operations must
be priced later. Promote only if both families have residual variance ratio
at most `0.08` and multilevel discrepancy MSE below `5e-7`. Otherwise kill
without changing the fraction.

