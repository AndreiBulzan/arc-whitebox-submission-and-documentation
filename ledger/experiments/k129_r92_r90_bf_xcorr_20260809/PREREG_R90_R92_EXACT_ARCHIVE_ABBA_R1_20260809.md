# Preregistration: isolated R90/R92 exact-archive ABBA

Date: 2026-08-09

Purpose: measure whether the frozen BF-XCORR readout's residual-time cost
erases its already sealed official-Mini100 accuracy gain.

Protocol:

- Hash-pin the exact R90 and R92 archives, their target-free Mini100 captures,
  and the capsule-native exact Mini100 worker.
- Hold `runtime/.benchmark_lane.lock` for the complete experiment.
- Execute archives in fixed `R90, R92, R92, R90` order.
- Each execution uses official Mini100 rows 0 and 1 in one persistent released
  pipe runner; row 0 initializes and row 1 is the steady measurement.
- Use CPUs 0--5 and six backend threads for every execution.
- Require exact initialized and steady FLOP counts and exact row-1 prediction
  hashes from the already sealed target-free package captures.
- Open no targets during this run.

Frozen economics:

- Official-Mini100 public raw ratio R92/R90: `0.9876649571164507`.
- The physical adjusted-factor gate is
  `raw_ratio * median_effective_R92 / median_effective_R90 < 1`.
- Report all request, residual, wall, and effective-compute deltas. Do not
  describe two rows per archive as broad statistical evidence.

Evidence boundary: each execution is a measured whole diagnostic; the ABBA
delta is measured-whole timing evidence. It is not remote evidence and it
does not authorize upload or submission.
