# Degree-6 adjoint support R1 verdict

Date: 2026-07-29

Evidence label: **component**.  This was a 16 Full + 16 Generated endpoint
atlas screen.  It used no physical row, FlopScope session, package, upload,
submission, or remote action.

## Result

The preregistered hard gate failed:

| family | pooled MSE ratio | row-ratio p95 | rows improved |
|---|---:|---:|---:|
| Full | 1.2909 | 2.2079 | 31.25% |
| Generated | 0.8939 | 1.8434 | 56.25% |

The target-free selector substantially reduced its own proxy objective on
every row, but that reduction did not predict the exact endpoint error.  The
mean-gated linear adjoint is therefore not a sufficiently faithful
network-specific degree-6 importance observable.  The Full/Generated
reversal also rules out treating the Generated central gain as a promotion
signal.

## Decision

Kill R1 exactly as preregistered.  Do not tune source rank, support size,
normalization, or greedy details on this opened screen.

The broader fixed-support result remains distinct: degree-6-aware static
geometry previously improved central error on both families but failed its
tail gate.  This experiment invalidates this particular weight-conditioned
adjoint router, not the degree-6 geometric observation itself.

Primary receipts:

- `degree6_adjoint_support_r1_targetfree_20260729.json`
- `degree6_adjoint_support_r1_postseal_20260729.json`

