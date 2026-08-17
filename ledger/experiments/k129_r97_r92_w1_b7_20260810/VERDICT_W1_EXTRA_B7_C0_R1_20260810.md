# Verdict: R92 W1 extra-B7 component C0/R1

Date: 2026-08-10

Verdict: **kill this contraction spelling before a whole run**.

Evidence boundary: exact FlopScope 0.10 **component** measurements and a
source-static live-production projection.  No target, whole estimator,
Mini100 row, package, upload, remote action, or submission was opened.

## What passed

The reduced rank-7 identity is numerically sound and cap-safe on all four
live representative geometries:

- prefix lane 1 and lane 2, `918x13 @ 13x13` with a reduced `12x12` core;
- recurrent lane 1 and lane 2, `918x12 @ 12x12`;
- relative RMSE versus direct matmul: `1.65e-7`--`1.76e-7`;
- maximum absolute displacement: `1.05e-5`;
- largest result: `82,047,168` bytes, below the 100 MiB cap.

The lane-1 and lane-2 charged counts scale exactly by independent logical
product, so the production count projection is not inferred from only one
batch size.

## Decisive failure

The algebraic B7 saving is `10,872` operations per logical product.  The
vectorized bank must then write the transformed operands.  FlopScope charges
those writes, reversing the count gain:

| live geometry | candidate minus direct per logical product |
|---|---:|
| prefix 13-column leaf | `+9,576` |
| recurrent 12-column leaf | `+8,658` |

Across the five prefix and eight recurrent layers this projects to:

```text
count delta                              +1,312,012,800
added public requests                           13,032
request-priced residual                         83.630 ms
effective-compute delta                  +9,675,025,525
```

The residual projection uses the frozen same-runtime R91 receipt price of
`5.8910725 ms / 918 requests`.  It is a **projection**, not a whole receipt;
the positive counted regression alone already fails the component gate.

## Why a trivial repacking cannot rescue it

For one public batched matmul, the seven left operands must coexist in one
regular bank.  Four derived operands can be written directly by the B7
add/subtract circuit, but the three unchanged raw `459x6` blocks still
require `8,262` charged writes per logical product.  Even granting free
right-bank preparation and every other layout benefit, the count-only upper
bound becomes:

```text
(10,872 - 8,262) * 145,600 = 380,016,000 saved
```

Writing the three raw right blocks lowers that ceiling to about `0.364B`.
Avoiding the bank instead requires separate matmul calls plus the B7
transform/reverse calls; the live 312-invocation surface then incurs enough
request residual to erase that sub-billion ceiling.  A fused two-digit
rank-49 circuit is a different compiler problem and remains governed by the
earlier request/add-throughput audits; it is not rescued by this component.

## Decision

Do not patch R92, run a whole ABBA, or spend Mini100 on this candidate.  The
failure is execution-economic, not numerical.  R92/R94/R95 remain unchanged.

Controlling receipt:

`runtime/artifacts/k129_r97_w1_extra_b7_component_c0_r1_20260810.json`

