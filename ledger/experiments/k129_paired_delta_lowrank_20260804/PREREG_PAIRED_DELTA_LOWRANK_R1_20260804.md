# Paired complete-frame delta low-rank R1 preregistration

Date: 2026-08-04

Evidence sought: **component** capacity evidence.  This pilot uses ordinary
CUDA, opens no challenge target during capture, and performs no FlopScope
row, package, upload, submission, or remote action.

## Question

Can a second complete K129 frame be represented as a low-rank correction to
the already-paid q0 particle trajectory, rather than propagated as another
full cloud?

For particle-matched q0 and right-Gram complete frames at each hidden layer,
write

```text
delta_l = h_right,l - h_q0,l.
```

Before each next affine map, replace `delta_l` by its target-free best
rank-`r` right-singular approximation, reuse the exact q0 preactivation, and
propagate only

```text
z_right,l+1 ~= z_q0,l+1 + delta_l,r @ W_l+1.
```

The pilot uses q0's static and late dynamic coordinate supports for both
paths.  That common-support restriction is part of the candidate and is
checked against the already-sealed independent complete-right endpoint.

## Prior-art boundary

Blocking searches covered low-rank/SVD particle states, paired trajectory
differences, alternate-frame width compression, and the exact K129 transport
sites.  The nearest controlled results are:

- `particle_subspace_contraction_20260728`: low-rank projection of the
  *entire incumbent particle state* failed even at rank 96;
- `k129_o1_shadow_width_20260729`: narrow neuron-axis propagation destroyed
  a sparse alternate arm; and
- `k129_paired_gate_innovation_20260804`: low-rank *basis-level aggregate
  gate features* did not reconstruct the complete-frame correction.

None tests the low-rank structure of the particle-matched **difference**
while leaving the full q0 state untouched.  Outcome: **materially new
observable**.  Failure here closes low-rank paired-delta transport; it does
not reopen full-state projection or width pruning.

## Frozen pilot

- Full rows: `17, 27`.
- Generated rows: `0, 1`.
- Frames: q0 and first-layer right-Gram.
- Ranks: `8, 16, 24, 32, 48`.
- Every low-rank approximation is the optimistic per-layer SVD oracle of the
  current target-free delta.  Challenge targets and complete endpoint errors
  are unavailable during capture.
- Final prediction: equal mean of exact q0 and the recursively approximated
  right-frame endpoint.  No fitted output coefficient.

The per-layer SVD is a capacity oracle, not a deployable operation.  A pass
licenses a separate structured-sketch/fixed-basis construction; it does not
license a score claim.

## Economic ceiling and gate

Remote R31 is `1.157747573e-7` at `141.140349B`.  A rank-16 paired correction
has a favorable dense-factor arithmetic projection around `25--35B`, so
sub-`1.1e-7` requires roughly a `24%` raw reduction.  Full q0+right reduces
official-Mini100 raw MSE by `40.9%`, leaving enough capacity if most of that
gain survives.

Promote only if one rank no larger than 24 satisfies all of:

1. pooled raw-MSE ratio to q0 `<=0.75` in both Full and Generated;
2. at least three of four rows improve;
3. common-support exact q0+right retains at least 90% of the independent
   full q0+right gain on each family; and
4. the recursively compressed correction recovers at least 60% of the
   common-support complete-frame gain in both families.

If no rank `<=24` passes, stop before a broader or Mini100 capture.  If one
passes, freeze the smallest passing rank and its deployable structured
spelling before the mandatory official Mini100 gate.

