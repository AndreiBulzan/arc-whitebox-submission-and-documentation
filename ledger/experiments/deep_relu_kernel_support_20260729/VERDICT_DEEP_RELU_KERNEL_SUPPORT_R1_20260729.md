# Deep ReLU-kernel support R1 verdict

Date: 2026-07-29

Verdict: **killed as an exact duplicate of the degree-6 support**.

## Target-free result

The CPU-only compact-geometry selector evaluated the antipodally
symmetrized normalized He-ReLU kernel composed exactly 31 times. Its lowest
17 orientation-1 bases were:

```text
3, 11, 15, 17, 27, 34, 36, 45, 54,
64, 66, 70, 78, 82, 111, 115, 120
```

This is byte-identical to the frozen degree-6 harmonic support: overlap
`17/17`, Jaccard `1.0`. Moreover, those degree-6 bases occupy deep-kernel
ranks exactly `1..17`. The deep-kernel selection-boundary gap is only
`2.93099e-14`; its candidate scores span merely
`0.9734283554558801 .. 0.9734283554592589`.

This is useful structural closure: at this geometry, antipodal symmetry and
the complete spherical-5-design O0 arm make the first nonconstant degree-6
harmonic determine the 31-layer ReLU-kernel ordering. The deeper kernel does
not produce a new support.

## Endpoint identity transfer

Per the preregistered fast-kill rule, endpoint and target arrays were not
reopened. Exact support identity transfers the already-sealed degree-6
signed-final-preactivation component result:

| family | pooled candidate/control | row-ratio p95 |
|---|---:|---:|
| Full100 | `0.9469685315` | `1.627754011` |
| Generated64 | `0.9499758603` | `1.634866417` |

Both pooled families improve by about 5%, but both miss the fixed R1 gate
of pooled ratio `<=0.90` and p95 `<=1.50`. No estimator successor is
licensed. Prospective runtime-operation delta remains `0B`.

## Evidence boundary

- Geometry selection: **component**.
- Endpoint metrics: **broad statistical component identity transfer**.
- No network weights or targets informed support selection.
- No GPU, FlopScope, physical/timed row, package, upload, submission, or
  remote action occurred.

## Seals

```text
preregistration
5814c19470dfe94a16418f615cdbd10d0bd55873662298b6011fc69c87000816

selector source
d67b3fd3d844ec7004df6e97dd947d5e7ad16c99031fc894e24f132e6ce7aca1

target-free NPZ
ff284df9ab630bb0b327134547803a20db705a1f3297c064c34c0406326baa88

target-free receipt
b1643f178c831174a978d3cf16d22d5b7dfc44556999dbf0f87521b230296e9e

identity-transfer scorer source
f189ec5fa8d59a02e32089e50528ed07f3309298aaf34a43608418b1a400db68

post-seal score receipt
9b9f7666413c75bb1256be6267780b611b097d5a62f8e180d0e91855d8250478
```
