# Paired gate-innovation R1 preregistration

Date: 2026-08-04

Evidence from this lane is **component** only.  No FlopScope session,
physical candidate, package, upload, or submission is authorized.

## Prior-work boundary

The following are already closed and will not be repeated:

- splitting the same 129 directions across several frames;
- endpoint, H2, depth-4, depth-6, and multidepth herding;
- literal sparse right and right/d2-right arms at sizes 12--68;
- static four-frame endpoint coresets;
- marginal covariance/K3/K4 continuation and weight-only routing.

This experiment introduces a different observable: particle-matched gate
disagreements and their signed nonlinear residual between q0 and the three
complete alternate frames.

## Frozen pilot

- Families/rows: Full `7,17`; Generated `0,1`.
- Frames: `q0`, `right`, `d2`, `d2right`.
- Checkpoints: depths `6,8,12,16,20,24`.
- Partial-frame sizes: `24,48,72,96,129` total alternate basis atoms.
- Selection methods, fixed before capture:
  `delta_mean`, `flip_mean`, `flip_rms`, `relevance_flip`, and
  `combined_relevance`.

For paired scalar preactivations `z0, za`, define

```text
flip = relu(za) - relu(z0) - 1[z0 > 0] * (za - z0).
```

This residual is exactly zero when the q0 and alternate trajectories remain
in the same ReLU cell.  Per-basis signed means, RMS values, gate-disagreement
fractions, and a target-free downstream squared-Jacobian relevance weighting
are captured.  Equal-weight herding uses these features only.  It may not
inspect complete endpoints, challenge targets, labels, row losses, IDs, or
scores.

The sealed partial prediction is

```text
q0 + 3/4 * mean(selected alternate endpoint - matched q0 endpoint).
```

Complete endpoints are used after selection only to materialize the already
chosen pilot prediction and to measure reconstruction of the target-free
four-frame teacher.  An endpoint-herding result is recorded separately as an
undeployable capacity ceiling and may not select the promoted method.

## Gates

Promote to a broader target-free capture only if one fixed method/size:

1. reduces q0-to-four-frame reconstruction MSE by at least 40% in both
   families (`ratio <= 0.60`);
2. does not depend on a different winning method in Full and Generated;
3. uses at most 48 alternate atoms, unless a larger result has a clearly
   superior projected accuracy/compute product; and
4. has either non-dense gate disagreement or a rank-16 basis-level response
   retaining at least 40% of flip-feature energy.

If no candidate passes, kill adaptive/feature-selected partial frames and
retain only the diagnostic question of whether a fixed mixed48 gate-
innovation receiver can outperform its current D6 diagonal continuation.

