# Mean-gated path-ReLU control R1 — hard kill

Date: 2026-07-29. Evidence label: **component**.

The non-smooth controls had exact sphere means and were cross-fitted, but
the mean-gated path proxy did not transfer to the true deep gate cells:

| K | Full ratio | Generated ratio | maximum row ratio |
|---:|---:|---:|---:|
| 16 | `3.802994` | `1.239522` | `4.285361` |
| 32 | `3.775526` | `1.985342` | `4.304705` |

The gate required `<=0.50` in both families and `<=1.25` per row.  The
failure is decisive.  Do not tune its ridge, feature count, gate
probabilities, or mean-Jacobian depth.  The projected incremental price was
below `35B`, but cheapness cannot rescue a wrong proxy.

The only justified successor in this control-functional class must obtain
directions from actual nonlinear gate cells, rather than another analytic
mean-gated path.

Authoritative score:
`path_relu_control_r1_postseal_20260729.json`.

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
