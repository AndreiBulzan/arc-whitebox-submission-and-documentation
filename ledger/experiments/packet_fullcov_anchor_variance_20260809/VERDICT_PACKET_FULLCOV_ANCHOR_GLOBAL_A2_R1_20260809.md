# Packet full-covariance anchor global A2/R1 verdict

Date: 2026-08-09

Verdict: **close the global shared full-covariance anchor-variance family**.

Evidence label: **component** on the sealed sixteen-network Gaussian-packet
population. No benchmark expectation target was opened.

## Result

The fixed Kerdock-spread prefix curve was evaluated after complete averaging
over all 66,048 oriented K129 centres:

| Antipodal pairs | Anchors | Pooled fidelity | Cosine | Unbiased residual MSE |
|---:|---:|---:|---:|---:|
| 1 | 2 | -497.75 | -0.0854 | 3.767e-5 |
| 2 | 4 | -249.97 | -0.0142 | 1.896e-5 |
| 4 | 8 | -159.44 | 0.0558 | 1.212e-5 |
| 8 | 16 | -110.56 | 0.0258 | 8.426e-6 |

The packet correction itself has unbiased energy `7.5534e-8`. Even sixteen
anchors miss it by more than two orders of magnitude and have essentially no
signed alignment. Both families fail independently. Literal-cloud
association is exact.

## Interpretation and boundary

The centre-marginal variance field is locally low-rank, but its small errors
produce a much larger spurious common mode than the true packet correction
after Kerdock/radial cancellation. Increasing a small fixed anchor prefix
reduces amplitude slowly without recovering the signed mode. A
score-compatible number of globally shared anchors cannot solve this.

This closes zero-, one-, and small fixed multi-anchor shared variance
schedules. It does not prove that a centre-dependent structured variance
field is impossible, but such a method must now demonstrate the global
common mode directly before any broad or production work.

No target, physical row, FlopScope session, package, upload, submission, or
remote action occurred.

