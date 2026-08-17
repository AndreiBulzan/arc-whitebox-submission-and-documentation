# Teacher-forced cross-neuron moment-message gate R2

Date: 2026-07-28

Evidence: **component**. This is a deliberately generous local-capacity
experiment, not an inference implementation, free rollout, FlopScope
physical row, candidate, package, upload, submission, or remote run.

## Question

R1 established that a local scalar view of a K32 cloud cannot predict its
official layerwise mean error. R2 tests the materially different missing
observable: cross-neuron pair geometry.

For transition `l=1..31`, give the model the exact official previous
post-ReLU mean and diagonal variance, together with current `W_l`. Start from
the diagonal-Gaussian ReLU closure and ask a cross-neuron message model to
predict only its local innovation:

```text
official_mean[l] - diagonal_gaussian(
    official_mean[l-1], official_variance[l-1], W[l])
```

This is teacher forced. No free rollout may be built unless the held local
gate passes.

## Equivariant message spelling

From normalized previous mean and standard deviation, form eight shared
source-neuron probes: location, scale, their centered forms, product,
centered squares, and centered-form product.

For all eight channels compute:

```text
S1 = W.T      @ H
Q1 = (W ** 2).T @ H
```

Include `S1`, `Q1`, and their elementwise squares. Then make one deterministic
two-hop pair-geometry round:

```text
B  = W @ tanh(S1) + (W ** 2) @ tanh(Q1)
H2 = tanh(B) + 0.5 H
S2 = W.T @ H2
Q2 = (W ** 2).T @ H2
```

The per-output readout receives these 48 message channels plus 12 analytic
diagonal-closure/current-column/global-state channels. A single
`60 -> 96 -> 96 -> 1` GELU MLP is shared across every layer, neuron, and
MLP. Its normalized correction is bounded by
`0.35 * tanh(logit) * diagonal_pre_sigma`.

No neuron identifier, MLP identifier, target-layer mean, or target-layer
moment is a feature. All maps are equivariant to a consistent permutation of
hidden neurons.

## Splits and the Generated variance caveat

Whole-MLP splits:

```text
Full train    0..199
Full dev      200..239
Full eval     240..319
Generated     0..63
```

Full higher-moment files provide exact previous official mean and diagonal
variance. The independent Generated bank contains official/MC all-layer
means but does not contain second moments. Its previous means are therefore
teacher forced while its diagonal variance is advanced by the same lawful
Gaussian moment recurrence, with the teacher mean injected between layers.
Generated is a stricter approximate-state transfer check, not a claim that
an unavailable exact variance was used.

Layer zero is excluded because it is analytically Gaussian.

## Fixed training

```text
readout              60 -> 96 -> 96 -> 1, GELU
optimizer            AdamW
steps                400
batch                 32768 neuron-transition records
learning rate         2e-3
weight decay          1e-4
seed                   2026072804
selection             Full-dev corrected/raw pooled absolute MSE ratio
feature clip          train-standardized +/-8
```

The model checkpoint is fixed using only Full train/dev MLPs. Full eval and
Generated are never used for fitting or checkpoint selection.

## Gate

Proceed to a free-rollout experiment only if all hold:

1. Full-eval corrected/raw local MSE ratio `<=0.25`;
2. Generated corrected/raw local MSE ratio `<=0.25`;
3. each depth quartile (over transitions `1..31`) is `<=0.40` on both;
4. predictions are finite.

Failure kills this exact cross-neuron message spelling and stops the lane.
