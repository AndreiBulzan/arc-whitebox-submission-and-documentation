# Polarized degree-6 regression cubature R1 — kill

Date: 2026-07-29

Verdict: **kill this exact control representation**.

Evidence label: **component**. This was a signed-final-preactivation screen
on 16 already-open Full and 16 process-separated Generated networks. It was
not a final-ReLU score, measured whole, FlopScope receipt, package, upload,
submission, or remote result. No physical/timed FlopScope row was run.

## Result

The primary target-free GCV rule failed at both support sizes:

| support | Full pooled ratio | Full p95 | Generated pooled ratio | Generated p95 |
|---:|---:|---:|---:|---:|
| `m=17` | `1.09896` | `1.30658` | `1.02170` | `1.26817` |
| `m=33` | `1.07224` | `1.23462` | `1.00735` | `1.19727` |

The response-weighted diagonal ablation was stable but reversed by family:

| support | Full pooled ratio | Generated pooled ratio |
|---:|---:|---:|
| `m=17` | `1.00690` | `0.96241` |
| `m=33` | `1.00121` | `0.95721` |

Larger polarized feature ranks worsened both families. Every GCV-selected
row chose the strongest ridge `alpha=1`; 30 of 32 rows chose rank 4. Thus
target-free reconstruction diagnostics themselves recognize that the mixed
degree-6 basis is mostly variance, not a missing interpolation degree.

## What was tested

R1 used the actual paid per-basis nonlinear endpoints and exact zero-mean
spherical harmonics. For each MLP it selected eight first-layer gate normals
by downstream response and built mixed sixth-order directions from every
pairwise sum and difference. Per-output ridge coefficients were fitted from
all 129 complete orientation-0 basis replicates; rank and damping were
selected by leave-one-basis-out GCV with no challenge target.

This is materially stronger than the killed degree-6 support router as a
within-rule control: it sees the nonlinear endpoint response on every paid
O0 basis instead of asking a linear adjoint to choose O1 identities. It
still cannot predict the signed integration error. The first unresolved
geometric degree is real, but its projection onto first-layer gate normals
is not the target-aligned deep alias.

Do not tune ridge values, add more polarized first-layer directions, score
another support size, or build a participant implementation.

## Current FlopScope 0.9.1 economics

All deployment economics here use only the current meter and remote
submission 320262 as the anchor (`170.3B` mean effective, adjusted
`1.315388e-7`). A direct rank-8 harmonic evaluation projects below about
`1.36B` ordinary dot-product operations for K162 before request/residual
tariffs; rank 4 is below `0.34B`. These are **projections**, not receipts.
The accuracy ratios are already above one on Full, so no current-meter score
case exists.

The mechanism also supplies no plausible low-K rescue. It does not meet even
an `8%` raw-improvement indication, much less the roughly `23%` raw
improvement needed for a K26-class sub-`1e-7` floor candidate.

## Seals

```text
preregistration
  9080904aafd2adc971d9ecd3bbe55f11182056ba76e0a44a91e65ec8cbc3596b

capture source
  537358fa48ca2749440d84b3e427ba58d291b7ce3656600639663288fe84cfba

target-free capture
  d4d29b2317dae01ba979ba8de577399c00420c176aa69e0f1261899f23606a22

post-seal scorer
  6ca464bc48524dd1b73895400de82d8953faa00a8014491950e7979d5c281f73
```

The authoritative score receipt is
`polarized_degree6_control_r1_postseal_20260729.json`.
