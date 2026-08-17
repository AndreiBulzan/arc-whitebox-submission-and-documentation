# Verdict: packet marginal-variance-field V1 R1

Status: **pass representational capacity**.

Evidence label: **component**. No target, physical benchmark, package, upload,
or submission was used.

The full-covariance packet closure was recomputed on the frozen four-network,
sixteen-centre micro teacher. At every layer its exact marginal preactivation
variance matrix was compressed across centres, then teacher-forced into a free
nonlinear mean rollout through all 32 supplied matrices.

The strongest result is rank zero: a single shared 256-vector per layer retains
`0.979016` of centre-averaged final correction fidelity for raw variance SVD,
with Full/Generated fidelities `0.976924` and `0.980833`, and pointwise fidelity
`0.951443`. The additive log representation reaches `0.984668`
centre-averaged at rank zero. Rank four is about `0.993` centre-averaged.

The sealed teacher recomputation error is exactly zero and rank-16 float64
rollouts agree within `1.6e-15`.

This passes the preregistered small-field gate. It does **not** show how to
compute the shared variance vectors lawfully. The authorized next experiment
is a target-free total-variance recurrence driven by already observable K129
canonical second moments.

Artifacts:

- preregistration:
  `PREREG_PACKET_MARGINAL_VARIANCE_FIELD_V1_R1_20260809.md`
- source:
  `run_packet_marginal_variance_field_v1_r1_targetfree_20260809.py`
- receipt:
  `runtime/artifacts/packet_marginal_variance_field_v1_r1_targetfree_20260809.json`
- arrays:
  `runtime/artifacts/packet_marginal_variance_field_v1_r1_targetfree_20260809.npz`
