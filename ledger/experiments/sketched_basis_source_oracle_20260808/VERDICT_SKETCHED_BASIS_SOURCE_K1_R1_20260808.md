# Sketched complete-basis source K1/R1 verdict

Status: **random output sketches preserve the `S=16` lead only at rank 128;
the intended rank-16/32 adjoint bridge fails**.

Evidence label: **component** on fixed Full640--641 and Generated88--89.
This new hypothesis was prompted by the earlier W1 development result, so it
is not an independent-family validation.  No Mini100, physical row, package,
upload, or remote action was used.

Eight fixed random orthogonal output sketches were tested at ranks
16, 32, 64, and 128.  Each sketch selected and signed-weighted 8 or 16 of the
129 complete-basis source vectors without benchmark expectation targets.

## Result

For per-row `S=16` fits trained on all packet replicates:

- rank 16: mean pooled raw reduction `-190.00%`;
- rank 32: `-4.75%`;
- rank 64: `24.41%`, with only 1/8 sketches passing 35%;
- rank 128: `37.60%` mean, `38.77%` median, range `30.83--44.15%`, with
  5/8 sketches passing the two-family 35% gate.

Rank-128 half-trained fits remain positive but lose material amplitude on the
opposite packet half: mean retained reductions are `22.48%` and `24.63%`.
Universal four-row sketches fail at every rank; rank-128 `S=16` retains only
`7.12%` pooled.

Every `S=8` configuration fails, including rank 128.

## Interpretation

The weighted 16-basis capacity is real, but its selection geometry is not
low-dimensional under generic output projections.  The intended
`r=16--32` bridge would require only `2,064--4,128` basis-projection scalars;
it is decisively inadequate.  The surviving rank-128 version requires
`16,512` basis-projection scalars and already exposes half the 256-dimensional
output geometry, before accounting for the cost of producing any projection.

Do not close structured or analytic basis-Gram contractions: a random
projection is not an optimal problem-specific sketch.  But do not propose
ordinary low-rank Hutchinson/JVP output sketches as the missing bridge.  The
next derivation must show how to apply or approximate the complete
basis-source Gram/total contraction collectively, rather than paying one
transport for every basis--probe pair.

Receipts:

- `runtime/artifacts/sketched_basis_source_k1_r1_targetfree_20260808.json`
- `runtime/artifacts/sketched_basis_source_k1_r1_postseal_20260808.json`

