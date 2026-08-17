# Verdict: packet total-variance cloud T1 R1

Status: **kill canonical-second-moment bridge**.

Evidence label: **component**, target-free. The run used the complete 66,048-row
K129 cloud on frozen Full8/Generated8 weights and compared with the sealed 64
replicate packet population. No benchmark targets or physical meter were used.

The dynamic total-variance recurrence is exact at layer 1. Its pooled packet
correction fidelity falls to `0.1077` at layer 2 and to `-42.4126` at layer 32;
the final cosine is `0.0101`. Full and Generated both fail. Q0 association is
exact.

This rejects `r2 * K129 canonical preactivation second moment` as the total
moment used to recover the shared marginal variance. It does not reject the V1
teacher result, which showed that the true shared marginal variance vector has
very high capacity. The next authorized bridge replaces the biased K129 total
with the separate analytic spherical second moments already computed by R90.

Artifacts:

- `PREREG_PACKET_TOTAL_VARIANCE_CLOUD_T1_R1_20260809.md`
- `run_packet_total_variance_cloud_t1_r1_targetfree_20260809.py`
- `runtime/artifacts/packet_total_variance_cloud_t1_r1_targetfree_20260809.json`
- `runtime/artifacts/packet_total_variance_cloud_t1_r1_targetfree_20260809.npz`
