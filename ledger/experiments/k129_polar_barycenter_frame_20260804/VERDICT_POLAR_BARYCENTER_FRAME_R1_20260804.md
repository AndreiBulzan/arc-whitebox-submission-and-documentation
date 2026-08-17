# K129 polar-barycenter frame R1 verdict

Date: 2026-08-04

## Decision

**Kill this exact class under its preregistered sub-`1e-7` gate.**  A single
polar frame preserved a nontrivial part of the complete-frame benefit, but it
did not transfer strongly or uniformly enough to become a candidate.

The candidate was selected only on Full16/Generated16.  Its pilot ratios were
`0.83029` and `0.70060`, but the unchanged official-Mini100 result was:

```text
candidate                         raw ratio       rows improved
polar(q0 + right + d2)            0.925818             53/100
paired 95% ratio interval         [0.7963, 1.0762]
required for sub-1e-7             <=0.86375            >=60/100
```

The best fixed Mini100 raw MSE was `2.379180746e-7`, versus
`2.569813399e-7` for q0.  At hypothetically identical effective compute this
would scale remote R31 to about `1.0719e-7`, but that is only a raw-ratio
projection: constructing the weight-conditioned frame has not been priced in
FlopScope, and the uncertainty interval includes no gain.

The polar inputs were also close to singular.  For the selected three-frame
candidate the minimum singular value had median `0.001406` and minimum
`1.79e-5`; the two-frame midpoints were still more singular.  Consequently a
small network change can rotate a weakly determined polar subspace sharply,
which is consistent with the pilot-to-Mini transfer loss.

This result is useful but not deployable: one complete orthogonal design can
carry roughly 7% raw improvement, proving that frame geometry matters, while
the literal Euclidean/polar barycenter is not a stable enough rule.  Reopen
only with a frame construction that controls the leading quadrature harmonic
or regularizes the near-null polar subspace by a target-free identity—not by
tuning more barycentric coefficients on Mini100.

Evidence:

- target-free capture:
  `runtime/artifacts/k129_polar_barycenter_frame_r1_targetfree_20260804.npz`
- post-seal score:
  `runtime/artifacts/k129_polar_barycenter_frame_r1_postseal_20260804.json`
- capture SHA-256:
  `700d23240b351d9d1554e13fee81400ff9dcd7ed62a5e4c6c02efb20982e5c49`

No FlopScope session, package, upload, submission, or remote action occurred.

