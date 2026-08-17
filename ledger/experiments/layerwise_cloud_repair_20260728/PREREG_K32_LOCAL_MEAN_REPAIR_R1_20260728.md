# K32 local mean-repair teacher gate R1

Date: 2026-07-28

Evidence: target-free feature capture, Full train/dev supervision, then
post-seal Full-eval and Generated scoring. This is a learned-capacity
**component** gate, not a FlopScope candidate, physical row, package, upload,
submission, or remote action.

## Question

Can a small, inference-lawful, hidden-permutation-equivariant local cell
predict the official layerwise ReLU-mean innovation of a 32-particle
structured cloud?

The primary gate is teacher-forced/local:

```text
MSE(raw cloud mean + predicted local correction, official mean)
---------------------------------------------------------------- <= 0.25
MSE(raw cloud mean, official mean)
```

on both held Full and independent Generated families.

Only if this passes may the learned correction be inserted into a free
32-layer particle rollout.

## Fixed cloud

Use eight uniformly spaced rows from the basis-zero rotation of each
production orientation and their exact antipodes:

```text
row IDs per orientation = 0,32,64,96,128,160,192,224
particles               = 2 orientations * 8 rows * 2 signs = 32
```

Particles are propagated literally in float32. Homogeneity restores the
analytic Gaussian chi-radius mean at every recorded post-ReLU mean.

## Inference-lawful feature family

For every MLP, layer, and output neuron, capture only:

- preactivation location, variance, absolute moment, skew and kurtosis;
- post-ReLU mean, variance and skew;
- two-arm mean/variance/gate/skew disagreement;
- the diagonal-Gaussian ReLU discrepancy;
- current weight-column norm, skew, kurtosis and signed sum;
- normalized layer index; and
- symmetric layer-global activation/disagreement/scale summaries.

Features use no target, other MLP, neuron identifier, or omitted particle.
A single scalar MLP is shared over every layer and neuron. Consequently the
map is equivariant to hidden-neuron permutations. It has no dense
width-by-width learned affine operator.

## Whole-MLP splits

```text
Full train    0..199
Full dev      200..239
Full eval     240..319
Generated     0..63
```

The Full bank is historically opened; no pristine claim is made. Capture
opens only weights. Training opens official all-layer means for Full
train/dev. Full-eval and Generated predictions are serialized and sealed
before their labels are opened.

## Fixed model/training

```text
features             22
shared MLP           22 -> 64 -> 64 -> 1, GELU
output               0.25 * tanh(logit) * local scale
optimizer            AdamW
steps                300
batch                 32768 neuron-layer records
learning rate         2e-3
weight decay          1e-4
seed                   2026072803
selection             Full-dev corrected/raw pooled MSE ratio
```

## Gates

Proceed to free rollout only if all hold:

1. Full-eval corrected/raw pooled MSE ratio `<=0.25`;
2. Generated corrected/raw pooled MSE ratio `<=0.25`;
3. every depth quartile ratio `<=0.40` on both families;
4. all predictions are finite; and
5. the archive/checkpoint/source hash chain is exact.

Failure kills this local feature/model spelling. It does not kill richer
coupled state, low-rank connected moments, or supervised models with a
genuinely new observable.
