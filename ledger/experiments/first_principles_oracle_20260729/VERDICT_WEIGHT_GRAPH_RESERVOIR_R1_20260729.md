# Weight-graph reservoir R1 — hard kill

Date: 2026-07-29

Evidence label: **broad statistical component**. Compute is a **projection**.
No FlopScope session, physical row, package, upload, submission, or remote
action was performed.

## Verdict

Kill the exact fixed all-layer weight-graph reservoir. It does not expose the
missing K146 final correction:

| locked family | corrected / baseline raw MSE | rows improved | row-ratio p95 |
|---|---:|---:|---:|
| Full held | `0.9861008` | `42.31%` | `1.12111` |
| Generated64, noise-corrected | `1.0032463` | `48.44%` | `1.17588` |

The preregistered continuation gate was a ratio at most `0.50` and at least
70% rows improved on both families. The selected ridge was already the
strongest shrinkage in the fixed ladder (`alpha=10000`), indicating that the
new channels carry essentially no transferable correction.

## Exact hypothesis tested

The state is weight-only and hidden-permutation equivariant over all 32
matrices. The first layer uses Gram features of `W0.T @ W0`, preserving the
Gaussian input's orthogonal invariance. Later layers use a fixed 24-channel
reservoir built from contractions through `W`, `abs(W)`, and `W**2` plus
source-permutation-invariant column statistics. One shared linear residual
head is fitted over output coordinates.

This distinguishes the falsifier from final-two-weight heads, moment
closures, particle-trajectory response, PTCC, support routing, and extra
quadrature. Its projected incremental work is below `1B`, so the failure is
statistical rather than economic.

## Lane conclusion

Together with this session's robust-basis-mean and basiswise-latent-K4 kills,
there is no surviving first-principles candidate in this lane with evidence
for halving raw MSE under the `272B` cap. In particular:

- trimmed and block-median basis reductions lose to the literal mean;
- the exact basiswise K4 spelling worsens MSE to `1.3858x` Full and
  `1.2173x` Generated;
- the published Gaussian-copula linearization collapses at the bias-free
  zero-mean operating point; and
- design-conditioned ReLU-boundary contraction was already physically killed
  by catastrophic source variance, so it was not rerun.

## Artifacts

- `PREREG_WEIGHT_GRAPH_RESERVOIR_R1_20260729.md`
- `screen_weight_graph_reservoir_r1_20260729.py`
- `weight_graph_reservoir_r1_existing_capture_20260729.json`

