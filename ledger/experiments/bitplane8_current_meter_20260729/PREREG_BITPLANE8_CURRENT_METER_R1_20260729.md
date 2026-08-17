# Eight-bit packed-cloud R1 preregistration

Date: 2026-07-29

Evidence sought: target-free **component** numerical precision and a static
FlopScope `0.9.1` **projection**.  This authorizes no physical/timed row,
package, upload, submission, or remote action.

## Question

Can per-particle activation and per-output weight 8-bit affine
quantization preserve the K64/K16 coupled cloud, and can an honest public
packed-bool or SWAR contraction reduce the current dense contraction cost?

This is not an accounting-omission search.  Every packing, bit operation,
population count, reduction, extraction, zero-point correction, and
materialization must be priced by the public `0.9.1` registry.

## Fixed numerical screen

Reuse the sealed K64/K16 emulator geometry and calibration without
retuning:

- Full row `0`, Generated row `2`;
- signed 8-bit activation and signed 8-bit weight quantization;
- clipping quantiles `0.001/0.999`;
- exact affine zero-point expansion;
- the existing fixed calibration shrinkage and `[0.5,2.0]` slope clip;
- quantize every ordinary layer (`eligible_all`) and, separately, layers
  `2..23` (`eligible_first24`).

No target is opened.  Report `Var(F-L)/Var(F)`, low discrepancy, and
multilevel discrepancy relative to the same-support dense cloud.

The precision gate is `r < 0.05` on both rows for at least one schedule.
The cost gate is an independently proved all-in cost no more than `0.70`
of dense.  Both must pass; numerical precision cannot rescue a
cost-regressing kernel.

## Static cost hypotheses

For an `M x 256` by `256 x N` product:

1. Direct `int8` matmul has the same dtype rate as float32 and therefore
   the same `511*M*N` contraction tariff before quantization.
2. A literal 8-by-8 bitplane dot has 64 plane pairs.  Even granting
   prepacked operands and ignoring uint64's `2x` dtype rate, each pair
   needs four-word AND, popcount, and reduction:

   ```text
   64 * 3 * 4 * M*N = 768*M*N > 511*M*N.
   ```

3. Honest Kronecker/SWAR packing can place two unsigned 8-bit digits in
   one uint64 lane with a 24-bit field, contract 128 packed positions, and
   extract the middle field.  The packed matmul bills:

   ```text
   dtype_rate(uint64) * (2*128 - 1) * M*N
   = 2 * 255 * M*N
   = 510*M*N.
   ```

   That is only break-even with dense before packing, extraction,
   zero-point correction, and dequantization.

The static audit will verify the public operation support and formulas.
If these lower bounds hold, the eight-bit deployment route is killed
regardless of its numerical screen.

