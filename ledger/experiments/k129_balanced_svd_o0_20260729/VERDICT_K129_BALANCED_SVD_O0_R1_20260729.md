# K129/O0 balanced-SVD support R1 — killed

Date: 2026-07-29.

Evidence label: **broad statistical** for the fixed Full100 and Generated64
accuracy comparison.  Compute and adjusted-score arithmetic are a
**projection**.  No FlopScope physical row, package, upload, remote action,
or submission occurred.

## Fixed candidate and integrity

The candidate used the exact preregistered phase-array positions
`1,5,9,...,125`, excluding the stored non-bent phase zero.  It retained K129
and endpoint lambda `0.0075`, replacing those 32 O0 phase frames by

`F = H @ U.T`,

where `U` is the canonical descending eigenframe of `W0 @ W0.T` and `H` is
the exact normalized order-256 Sylvester Hadamard transform.

The endpoint-patched CUDA control reproduced the sealed K129
lambda-0.0075 baseline byte-for-byte.  All 164 candidate predictions were
captured under the benchmark lock and sealed before target access.
All 164 frames passed:

- maximum float32-frame orthogonality error: `2.195699e-8`;
- maximum transformed-row-energy relative spread: `4.610053e-8`;
- required tolerance: `2e-6`.

The mathematical balancing property therefore held; the accuracy failure is
not a malformed-frame artifact.

## Broad result

| family | pooled raw ratio | row-ratio p95 | improved rows |
|---|---:|---:|---:|
| Full100 observed | `1.487741393` | `4.213494763` | `32/100` |
| Generated64 noise-corrected | `1.326470736` | `4.713682972` | `22/64` |

Using the nominal `+0.251985920B` count projection and the R21
remote-calibrated endpoint anchors gives projected adjusted scores of
`1.82881163e-7` and `1.63718622e-7`, respectively.

## Verdict

Kill this exact balanced-SVD O0 substitution.  Energy-equalizing the
left-singular frame worsens both families far more than the ordinary
right-eigenframe substitution.  Do not implement, package, or tune this
phase class from the opened banks.

Authoritative artifacts:

- `k129_balanced_svd_o0_r1_targetfree_20260729.npz`
- `k129_balanced_svd_o0_r1_targetfree_20260729.json`
- `k129_balanced_svd_o0_r1_postseal_score_20260729.npz`
- `k129_balanced_svd_o0_r1_postseal_score_20260729.json`

