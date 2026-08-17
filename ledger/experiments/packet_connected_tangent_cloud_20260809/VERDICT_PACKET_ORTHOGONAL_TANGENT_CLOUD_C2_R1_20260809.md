# Packet orthogonal-tangent cloud C2/R1 verdict

Date: 2026-08-09

Verdict: **close the first-Hermite connected-tangent/shared-marginal family**.

Evidence label: **component**, target-free.  No benchmark target, physical
row, FlopScope session, package, upload, submission, or remote action occurred.

The orthogonal construction repaired C1's controlling defect exactly.  Every
cell has layer-one correction fidelity `1.000` and cosine `0.985995`; Q0
association is `2.22e-16`.  Nevertheless the best `k8_r4` cell falls to
fidelity `-0.294` at layer two, `-6.273` at layer eight and `-42.620` at layer
32.  Its final cosine is only `0.1553`.  Smaller cells are no better.

This cleanly separates finite tangent quadrature from the covariance law.
The failure is caused by replacing the ReLU covariance with the first
Hermite/Bussgang term plus a diagonal birth.  Higher-order off-diagonal
boundary correlations are already controlling at layer two.  More tangent
probes or pilot bases do not repair that structural omission and are not
licensed.

The admissible successor changes the law: an explicit Price/Hermite covariance
series through orders 2--8, tested first as an overpowered shared-covariance
teacher.  C2 supplies no estimator or score claim.

Artifacts:

- `runtime/artifacts/packet_orthogonal_tangent_cloud_c2_r1_targetfree_20260809.npz`
- `runtime/artifacts/packet_orthogonal_tangent_cloud_c2_r1_targetfree_20260809.json`

