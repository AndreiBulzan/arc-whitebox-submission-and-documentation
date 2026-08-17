# Eight-bit packing on FlopScope 0.9.1 — hard kill

Date: 2026-07-29

Evidence labels: target-free **component** for the two-row trajectory
screen and **projection** for the public-meter cost proof.  No FlopScope
session, physical/timed row, package, upload, submission, or remote action
was used.

## Verdict

Eight-bit affine quantization is much better coupled to the dense cloud than
the prior 3/4-bit variants, but it still misses the preregistered precision
gate on Generated and—independently—has no lawful cost advantage under the
current `0.9.1` meter.

| schedule | Full `Var(F-L)/Var(F)` | Generated `Var(F-L)/Var(F)` |
|---|---:|---:|
| all eligible layers | `0.035762` | `0.066919` |
| layers 2 through 23 | `0.035914` | `0.061410` |

The fixed numerical gate was `<0.05` on both families.  The screen opened no
target data.  It therefore says that int8 is a plausible numerical storage
format, not that it is a cheap estimator kernel.

The cost gate fails before implementation:

```text
dense float32 or direct int8 matmul             511 * M*N
8x8 literal bitplanes, generous lower bound     768 * M*N   (1.503x)
two-int8 uint64 SWAR matmul, before overhead     510 * M*N   (0.998x)
```

The bitplane lower bound grants prepacked inputs, ignores the public
`uint64` rate of `2`, and omits quantization, extraction, weighting,
accumulation, movement, and residual.  It is already 50.3% more expensive
than dense.

The only carry-safe two-int8 Kronecker/SWAR construction needs roughly
24-bit fields for a length-256 dot.  It packs two digits, halves the inner
dimension to 128, then loses that entire saving to the `uint64` rate:
`2*(2*128-1)=510`.  All necessary surrounding work makes it a regression.
Packing more 8-bit digits cannot retain the required middle coefficient
without cross-field carries in 64 bits.

## Public support and operational surfaces

This route is lawful and in-bounds, not an exploit.  Public `0.9.1`
supports `bitwise_and`, `bitwise_count`, shifts, `packbits`, `unpackbits`,
`sum`, and `matmul`; each operation is charged by its published rule.
Direct int8 matmul receives the same rate as float32.

It also has an unfavorable wire/result shape.  A broadcast AND/popcount
carrier has four packed words per dot-product pair, so it must be segmented
to remain below the ordinary `104,857,600`-byte single-result cap on the
large cloud blocks.  Segmentation cannot improve the arithmetic lower
bound and adds requests and residual.

## Decision

Do not build a FlopScope bitplane or int8-SWAR cloud kernel.  Reopen
quantization only if it changes the statistical representation—for example,
a small correction cloud coupled to a different low-cost primary
estimator—not as an alternative spelling of the present dense
contractions.

Authoritative artifacts:

- `bitplane8_precision_r1_targetfree_20260729.json`
- `bitplane8_static_cost_r1_20260729.json`

