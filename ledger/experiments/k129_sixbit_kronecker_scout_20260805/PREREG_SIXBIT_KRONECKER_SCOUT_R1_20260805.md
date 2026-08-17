# Six-bit Kronecker/SWAR contraction R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical fidelity and an exact
static counted-cost **projection** under FlopScope 0.10.0. This authorizes no
physical row, scoring, package, upload, submission, or remote action.

## Prior-work check

Capsule searches for `six-bit`, `6-bit`, `int6`, `three packed`, `SWAR`, and
`Kronecker` found no six-bit implementation or verdict. The nearest work is:

- the four-bit calibrated cloud scout, killed by numerical coupling; and
- `bitplane8_current_meter_20260729`, where eight-bit fidelity was close but
  honest two-term uint64 packing was statically break-even.

This test differs discretely from both. With unsigned affine codes in
`[0,63]`, a length-256 encoded dot is at most
`256 * 63 * 63 = 1,016,064 < 2^20`. Three terms therefore fit in a uint64
Kronecker lane with radix `B=2^20`:

```text
A = a0 + B*a1 + B^2*a2
W = B^2*w0 + B*w1 + w2
```

The coefficient of `B^2` is the wanted three-term dot. Across 86 packed
positions it remains below `B`; terms at bit 60 and above cannot contaminate
bits 40--59 even when uint64 arithmetic wraps. The dense contraction tariff
per scalar output is `2*256-1 = 511`; the packed uint64 tariff is
`dtype_rate(2) * (2*86-1) = 342`, or `0.669276` of dense before transforms.
All packing, extraction, affine correction, and dequantization must later be
charged; this is not an accounting-omission or residual-compute route.

## Fixed fast numerical falsifier

Reuse the sealed K64/K16 calibrated-cloud emulator without retuning:

- Full development row 0 and Generated row 2;
- unsigned six-bit affine activation and weight codes;
- clipping quantiles 0.001/0.999;
- exact affine zero-point reconstruction;
- schedules `eligible_all` and `eligible_first24`;
- compare against the same-support dense float32 cloud;
- no target access and no FlopScope execution.

The script must also verify the three-term uint64 identity on deterministic
random integer matrices before touching a network.

## Decision

Promote to a 4+4 target-free component screen only if at least one schedule
has `Var(F-L)/Var(F) <= 0.08` on both families and multilevel discrepancy MSE
below `5e-7` on both. This is deliberately a permissive first screen: an
actual deployment still needs a fully charged cost model and a stricter
allocation proof. Otherwise kill the six-bit lane immediately.

