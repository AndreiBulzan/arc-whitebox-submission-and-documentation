# Great-circle quadrature R1 verdict

Date: 2026-07-29

Verdict: **killed by the preregistered cross-family gate**.

Evidence label: **component**. This is a target-free capture followed by a
post-seal 4 Full + 4 Generated score. It is not broad statistical evidence,
a measured whole, or a remote result.

## Result

Every method propagated exactly 4096 unit-sphere directions and used the same
exact layer-1 radial first/second-moment repair.

| support | Full final MSE | ratio | Generated final MSE | ratio |
|---|---:|---:|---:|---:|
| Kerdock K8 control | 2.1359e-6 | 1.000 | 6.5104e-6 | 1.000 |
| Haar circle q4 | 3.0056e-6 | 1.407 | 3.6163e-6 | 0.555 |
| Haar circle q8 | 5.9901e-6 | 2.804 | 3.8988e-6 | 0.599 |
| Haar circle q16 | 5.0086e-6 | 2.345 | 1.2162e-5 | 1.868 |
| Haar circle q32 | 9.3130e-6 | 4.360 | 1.4757e-5 | 2.267 |
| Kerdock-plane circle q8 | 4.1206e-6 | 1.929 | 6.0552e-6 | 0.930 |
| Kerdock-plane circle q16 | 6.3592e-6 | 2.977 | 1.2899e-5 | 1.981 |
| Kerdock-plane circle q32 | 1.2961e-5 | 6.068 | 1.5628e-5 | 2.401 |

No method met the continue gate (`<=0.90` on both families); the best
cross-family maximum ratio was `1.4071`. The apparent q4/q8 Generated gain
reverses substantially on Full and cannot be promoted.

## Mechanism conclusion

Uniform angular polygons do reduce conditional angle noise, but at fixed
support they replace independent two-plane coverage with repeated evaluations
inside the same plane. At depth 32, the between-plane contribution dominates.
Increasing polygon order therefore reduces the number of represented planes
faster than it resolves the many ReLU-induced angular breakpoints. The
monotone deterioration at high q occurs for both independent Haar frames and
production Kerdock frames, so it is not a bad-basis artifact.

The supports all passed exact isotropy checks: empirical covariance relative
Frobenius errors were `1.28e-8` to `3.85e-8`. This rules out second-moment
imbalance as the explanation.

## Budget conclusion

At equal row count, these supports would have the same first-order dense cloud
matrix-product cost as Kerdock. That lawful feasibility is moot because they
lose accuracy. Do not spend a physical, packaging, or broad-statistical lane
on this family without a new mechanism—specifically, a weight-conditioned
plane choice or an analytic treatment of the *entire* angular piecewise-linear
partition, not merely a higher fixed polygon order.

## Seals

- capture source SHA-256:
  `6b11ec7bd888fa14d4d163c065aefde1792060ff5b60a723be66ee8334c26c2a`
- preregistration SHA-256:
  `b61a218e2ec03da77493947d9e5b42fe1ff3cba60e9fdf2a27df1181e4be7985`
- target-free capture SHA-256:
  `f588e50e0178581741a300128ba74d3c9d5f418357df4a36eec8a1553256a672`
- post-seal scorer SHA-256:
  `ed3db72eebbdfe2a52c0a475ef38107b49dca2f078f24d102e0ec8f140bd8f7f`

Artifacts:

- `great_circle_quadrature_r1_targetfree_20260729.npz`
- `great_circle_quadrature_r1_targetfree_20260729.json`
- `great_circle_quadrature_r1_postseal_20260729.json`

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.

