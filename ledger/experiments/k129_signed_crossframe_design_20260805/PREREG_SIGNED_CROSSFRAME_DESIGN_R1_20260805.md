# Signed cross-frame design compression R1

Date: 2026-08-05

Evidence scope: target-free **component** screen on Full rows `7,17` and
Generated rows `0,1`.  No challenge target, FlopScope session, physical row,
package, upload, or submission is permitted.

## Motivation and distinction

The official Mini100 capacity receipt shows that the equal mean of the q0,
right-Gram, depth-2 `J J^T`, and depth-2 `J^T J` complete K129 frames has
`0.3544x` the raw MSE of q0.  Equal-weight partial-frame and nonnegative
endpoint coresets did not preserve this cancellation.

Dillon's fixed-strength spherical-design construction (arXiv:2502.06002,
v3) highlights a different degree of freedom: signed weighted designs can be
substantially smaller than ordinary positive designs.  R1 tests that degree
of freedom on our *network-conditioned union* rather than constructing a new
generic design.

For every one of the four frames, reconstruct its 129 complete orthonormal
bases.  On fixed target-free spherical probes, each basis contributes its
centered degree-4 moment.  Select exactly 129 bases across all four frames and
fit signed weights subject to exactly one quarter of the total mass coming
from each frame.  Selection and weights may use only frame geometry.  They may
not use endpoints, predictions, or challenge targets.

This is not the killed quartic R4 rule: R4 selected 48 equal-weight alternate
bases greedily.  It is not the static coreset: that rule was nonnegative,
endpoint-trained, fixed across networks, and used only q0/right.  R1 is a
dynamic, geometry-only, four-frame, signed fully-corrective rule at one-frame
support.

## Frozen rule

- 512 fixed training probes and 512 disjoint held probes, seed
  `2026080501`.
- Start with the minimum training-feature-norm basis in each frame.
- Repeatedly solve the equality-constrained ridge problem on the selected
  support, then add the unselected atom with largest absolute reduced
  gradient within its frame.
- Support size: 129 bases total.
- Ridge: `1e-10` times the mean diagonal feature Gram.
- Per-frame signed mass: exactly `0.25`.

The expensive four-frame endpoint banks are opened only after the support and
weights for a row have been fixed.  They are labels for teacher-reconstruction
capacity, not selector inputs.

## Promotion gate

Promote to a wider/Mini100 candidate capture only if all conditions hold:

1. mean teacher-reconstruction MSE ratio to q0 is at most `0.60` in both
   families;
2. every row's ratio is at most `0.90`;
3. signed-weight variance inflation `129 * sum(w**2)` is at most `3.0` on
   every row;
4. `sum(abs(w))` is at most `2.0` on every row;
5. the held-probe degree-4 RMS defect is finite and no more than `1.5x` its
   training value plus `1e-10`.

Failure kills this exact sparse signed quartic construction.  It does not
kill other harmonic strengths or a new node family.

