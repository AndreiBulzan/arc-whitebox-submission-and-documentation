# K146-anchored source-residual corrector R1

Evidence target: **component**. This is an offline accuracy screen. It does
not authorize a physical run, package, upload, or submission.

## Hypothesis

Keep the frozen K146/m17 prediction as the primary estimator. At the input
to the final weight matrix, the two existing orientation arms already expose
two estimates of the layer-30 activation moments. A small shared source-node
map may predict part of the error in their blended layer-30 mean. Transport
that correction through the real final weight matrix and the analytic final
ReLU gate.

The deployed map must be:

- equivariant to permutations of layer-30 nodes and final outputs;
- invariant/equivariant under positive hidden-node gauge rescaling;
- target-free at inference;
- an additive correction to K146, never a standalone mean predictor; and
- statically projected at no more than `5e9` additional operations.

## Fixed data and split

Use the already frozen K146 Full100 / Generated64 row manifests. In their
stored order:

- Full positions `0:60`: training;
- Full positions `60:80`: model/step/shrinkage selection;
- Full positions `80:100`: sealed network holdout;
- all Generated64: sealed family holdout.

The acquisition may observe the two layer-30 arm clouds and final readout
moments, but must not open any target. The fit may open only Full positions
`0:80`. It must serialize predictions for Full positions `80:100` and
Generated64 before either evaluation target is opened.

## Fixed feature/mechanism family

For each layer-30 source node, use dimensionless arm mean/variance/K3/K4
features, their disagreements, gauge-normalized invariants of its outgoing
final-weight row, and row-global averages of those dimensionless features.
A shared pointwise MLP predicts a dimensionless source-mean correction.
Multiply by the local activation scale, transport once through the actual
final weight matrix, and multiply by the target-free analytic final gate.

Training combines exact Full teacher loss on the layer-30 mean correction
with end-to-end final residual loss. Candidate selection may use only the
Full20 validation split. A scalar shrinkage chosen on that validation split
is permitted; no Generated quantity may select it.

## Hard early gate

Promote only if the frozen candidate achieves all of:

- at least `12%` pooled raw-MSE reduction on sealed Full20;
- at least `12%` pooled noise-corrected raw-MSE reduction on Generated64;
- finite predictions;
- candidate maximum row MSE no more than `1.25x` baseline maximum in each
  bank; and
- projected additional deployment operations no more than `5e9`.

Otherwise stop this spelling without tuning or widening it.

