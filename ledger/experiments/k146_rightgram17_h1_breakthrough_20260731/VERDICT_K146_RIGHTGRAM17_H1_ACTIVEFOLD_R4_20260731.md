# K146 right-Gram17 + H1 + active-fold R4 verdict

Date: 2026-07-31

R4 is the new local submission candidate. It is not a remote result and it
has not been authorized for upload.

The estimator adds 17 target-free, weight-conditioned directions from the
canonical eigenframe of `W0.T @ W0` to the K129 primary cloud. It combines
that accuracy mechanism with the exact repaired-H1 contraction and a
prediction-preserving vector rewrite of the two hottest view families.

Broad statistical accuracy transfers across both named families:

| bank | raw ratio versus R26 | raw gain |
|---|---:|---:|
| Full100 | 0.802147 | 19.785% |
| Generated128 | 0.807042 | 19.296% |

The exact archive passed the official LocalRunner lifecycle for one
initialized and two steady predictions. Both steady rows used
`143,442,663,612` counted FLOPs; their effective compute was `158.706 B` and
`158.885 B`. All three predictions were byte-identical. Bare setup issued
zero FlopScope requests.

The active layout rewrite removed 5,664 steady `__getitem__` requests versus
R2 while preserving prediction bytes. The repeated steady residual was
`0.1526--0.1544 s`, down about `0.0240 s` versus R2, for a measured local
effective saving of about `2.401 B` or `1.49%`.

Calibrated from remote submission 321393 and the respective local
steady-effective values, the projected adjusted score is:

```text
Full100       1.08553e-7
Generated128  1.09216e-7
```

This clears the `1.1e-7` checkpoint as a projection on both broad families
and is approximately 9.0--9.6% better than the `1.2004426e-7` remote anchor.
The remaining uncertainty is remote runtime/score transfer; this wording
must not be upgraded to remote evidence before an actual grade.

Pinned archive:

```text
runtime/artifacts/k146_rightgram17_h1_vectorviews_activefold_local_candidate_r4_20260731.tar.gz
sha256 0b775f0c68e622d3e3ab5575caeeb2b9cdbd538fda9227568b664f0a8e237890
```

Primary receipt:

```text
runtime/artifacts/k146_rightgram17_h1_vectorviews_activefold_projection_r6_20260731.json
```

No upload, submission, or other remote action was performed.
