# Phase-interlaced random-moment R3 verdict

Date: 2026-08-06

Verdict: **killed at the preregistered 4+4 component gate.**

The target-free balanced pair-swap rule reduced its 64 fitting-probe loss by
about 95%, but did not transfer to the disjoint 64 validation probes:
optimized/cyclic validation loss was `1.1952` on average and `1.3862` at
worst, versus the required `<=0.25` on every row.

After the capture was sealed, the literal same-K129 mixed rule gave raw-MSE
ratios to q0 of `0.8933` on Full4 and `1.8905` on Generated4.  It improved
only `1/8` rows and its worst row ratio was `2.7736`.  All four gates failed.

This closes random directional sketches, alternative probe seeds/counts,
and pair-swap restarts as a way to claim restoration of the fourth moment.
It does not test the exact fourth-order frame-potential objective, whose
pairwise basis-overlap formula has no projection sketch.

Evidence:

- target-free capture: `runtime/artifacts/k129_phase_interlaced_moment_r3_targetfree_20260806.npz`
- target-free report: `runtime/artifacts/k129_phase_interlaced_moment_r3_targetfree_20260806.json`
- post-seal score: `runtime/artifacts/k129_phase_interlaced_moment_r3_postseal_20260806.json`

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.
