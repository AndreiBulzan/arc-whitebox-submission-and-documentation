# Basis-level nonlinear gate-cell control R1 — hard kill

Date: 2026-07-29. Evidence label: **component**.

The statistically corrected basis-replicate fit is catastrophically
ill-conditioned and closes this control-functional seam:

| K | Full ratio | Generated ratio | maximum row ratio |
|---:|---:|---:|---:|
| 16 | `13885.4237` | `4427.7450` | `15268.4770` |
| 32 | `15666.6825` | `10231.6159` | `20449.0543` |

The four basis-control deviations are too small and too weakly coupled to
the 256-vector output error to identify a stable per-MLP coefficient from
8--16 training replicates.  Cross-fitting exposes rather than hides that
failure.  The projected incremental cost was below `2B`, but no further
ridge, rank, basis split, or direction scan is justified.

Together with the exact H1-quadratic, mean-gated path-ReLU, and pointwise
actual-gate-cell results, this closes the tested known-mean
control-functional class.  None approached the required two-family
`0.50x` gate.

Authoritative score:
`basis_gatecell_control_r1_postseal_20260729.json`.

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
