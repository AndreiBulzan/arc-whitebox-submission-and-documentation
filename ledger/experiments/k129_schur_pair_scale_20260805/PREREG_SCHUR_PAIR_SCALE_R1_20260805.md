# Schur-pair single output-scale transfer R1

Date: 2026-08-05

Evidence sought: **broad statistical**.  This is a post-seal scalar analysis
of existing target-free predictions.  It authorizes no physical run,
package, upload, or submission.

## Prior-work boundary

The deployed q0 lineage already carries the globally frozen output scale
`1.000025`.  A repository-wide search found no output-scale recalibration of
the new gamma=.5 Schur-pair frame.  No rowwise, family-specific, per-neuron,
or MLP-identity-dependent coefficient is permitted.

## Frozen selection and confirmation

Let `p` be the already sealed gamma=.5 prediction, which already includes
the inherited scale.  On the pooled Full100 and Generated128 selection bank,
choose the unique least-squares multiplier

`t = sum(p * target) / sum(p * p)`.

The deployed scale would be `1.000025 * t`.  Generated label-noise correction
does not affect this minimizer because it is constant in `t`.  Apply this
single scalar unchanged to official Mini100.

Promote only if:

- `abs(t - 1) <= 5e-4`;
- aggregate raw MSE strictly improves on Full100 and Generated128 separately;
- official Mini100 raw MSE strictly improves;
- at least half of Mini100 rows improve; and
- the Mini100 improvement is at least `0.15%` so float and sampling noise do
  not masquerade as a useful deployment change.

Failure kills this exact scalar retuning.
