# K129 mixed64/D6 q0-anchor coreset — R4 preregistration

Date: 2026-08-03

## Question

The correct R5 feature subtracts the mean of 129 q0 depth-6 diagonal
continuations from each of 64 alternate continuations.  Capturing all q0
depth-6 moments is cheap because the q0 cloud is already paid, but continuing
all 129 basis states is not.  Can a fixed affine coreset of q0 basis states
reproduce the anchor closely enough to keep the broad accuracy gain?

## Prior-art boundary

The search covered `q0 proxy mean`, `affine prune`, `proxy coreset`, `paid
anchor`, and `D6 support`.  Existing four-frame coreset work pruned complete
frame endpoints or selected alternate atoms.  R13's paid-q0 endpoint anchor
and this session's literal paid-q0 R2 both failed.  No result compresses the
complete q0 D6 proxy mean specifically as the reference for the independently
repaired mixed64 feature.  This is therefore a new observable, using the
already audited affine-pruning primitive.

## Frozen target-free selection

- Candidate q0 support sizes: `8, 16, 24, 32, 48, 64`.
- For each size, backward affine pruning uses the family-balanced Gram of the
  centered, sealed q0 D6 basis proxies.  Ridge-relative is `1e-6`.
- The alternate 64 slots, frames, bases, directions, depth, and literal
  repair are unchanged.
- For each anchor, refit the 64 correction coefficients at ridge `1.0` with
  the existing four-fold family-balanced target-free teacher objective.
- Select the minimum of worst-family OOF reconstruction MSE times projected
  effective-compute ratio; ties favor fewer q0 bases.
- Seal the selected predictions before opening challenge targets.

## Gates

Promote only if post-seal broad scoring improves over q0 on both families and
the worse central adjusted projection is at most `1.10e-7`.  The compute
projection charges `(64 + q0_support)/193` of the former 1.4B diagonal
allowance, plus R31 and the exact 4.085645312B unique-frame saving.  A promoted
result still needs a whole physical ledger.  Failure kills the entire q0
anchor-coreset spelling without another support/ridge search.
