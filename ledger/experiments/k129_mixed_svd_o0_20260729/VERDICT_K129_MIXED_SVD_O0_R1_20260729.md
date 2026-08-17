# K129/O0 mixed-SVD support R1 — killed

Date: 2026-07-29.

Evidence label: **broad statistical** for the fixed Full100 and Generated64
accuracy comparison.  Compute and adjusted-score arithmetic are a
**projection**.  This work performed no FlopScope physical row, package,
upload, remote action, or submission.

## Fixed candidate and integrity

The exact current K129/O0 trajectory was retained at endpoint lambda
`0.0075`.  Of its 128 nonzero phase bases, positions
`0,4,8,...,124` used the canonical descending eigenframe of `W0.T @ W0`;
basis zero and the other 96 phases retained the incumbent orientation.
There was no phase, fraction, endpoint, or family grid.

Before the candidate capture, the endpoint-patched CUDA adapter reproduced
the first exact sealed K129 lambda-0.0075 baseline vector byte-for-byte.  The
164 candidate predictions were captured from weights only while holding the
shared benchmark lock, were finite, and were sealed before target access.

## Broad result

| family | pooled raw ratio | row-ratio p95 | improved rows |
|---|---:|---:|---:|
| Full100 observed | `1.144915752` | `2.273044665` | `42/100` |
| Generated64 noise-corrected | `1.011764267` | `2.901086467` | `32/64` |

The fixed substitution worsens both broad families, catastrophically so on
Full100.  It fails the encouraging `<=0.98` ratio screen and the
compute-conditioned checkpoint threshold of approximately `<=0.9722`.

Using the nominal `+0.251985920B` operation projection and the R21
remote-calibrated endpoint anchors gives projected adjusted scores of
`1.40739194e-7` on Full100 and `1.24876220e-7` on Generated64.  A
conservative direct-operation upper bound of `+0.255655936B` changes these
only to `1.40742702e-7` and `1.24879333e-7`.

## Verdict

Kill this exact same-K O0 mixed-SVD support.  Do not implement it in
FlopScope, package it, or tune its phase fraction from these opened banks.
The earlier O1 mixed-SVD improvement does not transfer when one quarter of
the dominant O0 support is replaced.

Authoritative artifacts:

- target-free capture:
  `k129_mixed_svd_o0_r1_targetfree_20260729.npz`
- target-free manifest:
  `k129_mixed_svd_o0_r1_targetfree_20260729.json`
- post-seal arrays:
  `k129_mixed_svd_o0_r1_postseal_score_20260729.npz`
- post-seal receipt:
  `k129_mixed_svd_o0_r1_postseal_score_20260729.json`

