# Seven-bit blocked Kronecker contraction R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical fidelity and a static
FlopScope 0.10.0 cost **projection**. No physical row, target score, package,
upload, submission, or remote action is authorized.

## Prior-work boundary

Capsule searches found four-bit and eight-bit quantized-cloud verdicts and
the new six-bit three-way global packing verdict, but no seven-bit, blocked
carry-reset, or batched four-term Kronecker construction. The eight-bit audit
proved that a *single full-length* packed dot is break-even; this spelling is
different because it extracts before carries accumulate across the full
contracted dimension.

## Exact arithmetic hypothesis

Encode affine codes in `[0,127]`. Partition the length-256 contraction into
64 groups of four. For radix `B=2^16`, pack each group as two contracted
lanes:

```text
A0 = a0 + B*a1       W0 = B*w0 + w1
A1 = a2 + B*a3       W1 = B*w2 + w3
```

In `A0*W0 + A1*W1`, the low coefficient is at most
`2*127^2 = 32,258 < B`, and the wanted middle coefficient is at most
`4*127^2 = 64,516 < B`. Thus bits 16--31 are the exact four-term encoded
dot. Terms beginning at bit 32 may wrap in uint32 without contaminating it.

Batch the 64 independent two-lane contractions in one call. Per scalar
output, before packing and affine reconstruction:

```text
64 batched two-lane dots       64 * (2*2-1) = 192
shift and mask                 64 * 2       = 128
uint32 group reduction         64 - 1       =  63
total                                           383
dense float32 dot                               511
```

This is a `0.7495x` contraction core, not a final all-in claim. A promotion
requires the public meter to charge every pack, reshape, extraction,
reduction, zero-point correction, scale, and movement operation.

## Fixed fast numerical falsifier

Reuse the sealed K64/K16 calibrated-cloud emulator without retuning:

- Full row 0 and Generated row 2;
- unsigned seven-bit affine activation and weight codes;
- clipping quantiles 0.001/0.999 and exact zero-point reconstruction;
- schedules `eligible_all` and `eligible_first24`;
- same-support dense float32 cloud as control;
- no target access and no FlopScope execution.

Before network work, verify the uint32 construction against direct uint64
dots on deterministic random arrays.

Promote to the 4+4 target-free screen only if one schedule has residual
variance ratio at most `0.08` and multilevel discrepancy MSE below `5e-7`
on both families. Otherwise kill immediately.

