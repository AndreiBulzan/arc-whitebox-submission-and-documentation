# Nonlinear-pilot gate-cell ReLU control R1 — kill

Date: 2026-07-29. Evidence label: **component**.

Actual deep gate-cell gradients were materially better than the killed
mean-gated proxy, but did not transfer reciprocally:

| K | Full ratio | Generated ratio | maximum row ratio |
|---:|---:|---:|---:|
| 16 | `1.090887` | `0.828254` | `1.461346` |
| 32 | `1.164421` | `0.989496` | `1.191943` |

One Generated row reached `0.575376x`, proving the controls are not
numerically inert.  The unchanged rule nevertheless worsened both Full
rows and failed the `<=0.50` two-family gate.  The projected incremental
cost was below `36B`.

The pointwise regression objective is now closed.  One final distinct
falsifier may fit the control at the complete-basis replicate level, where
the structured quadrature error actually lives.  Do not tune the R1
pointwise ridge, rank, pilot basis, or adjoints.

Authoritative score:
`gatecell_relu_control_r1_postseal_20260729.json`.

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
