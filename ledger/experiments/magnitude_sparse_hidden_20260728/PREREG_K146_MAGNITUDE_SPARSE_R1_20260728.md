# K146 magnitude-sparse hidden contractions R1

Date: 2026-07-28  
Evidence: target-free capture plus `component` post-seal score.

## Fixed question

Prior width-compression lanes failed by deleting hidden state dimensions.
R1 instead retains every neuron in the existing analytic live screens and
sparsifies only the aligned hidden weight matrix. For each output column,
it keeps the largest absolute weights and rescales the retained column to
the original squared norm:

```text
S_j = top-k_i |W_ij|
scale_j = sqrt(sum_i W_ij^2 / sum_{i in S_j} W_ij^2)
W_sparse[i,j] = scale_j * W[i,j] if i in S_j else 0
```

The selection and rescaling depend only on the submitted MLP weights.
No target, score, fitted coefficient, row routing, or exploit is involved.
R1 deliberately adds no mean offset so that it tests the specified
variance-renormalized mechanism without a second moving part.

The component replay uses dense masked Torch matrices solely to measure
accuracy. It is not a wall or sparse-runtime receipt.

## Fixed grid

The live screen is 232 wide through layer 20 and 200 at layer 21.

```text
mean_s12_w216       associated incumbent control
dense_live_w232     no width deletion and no sparsification
sparse_s12_f0750    layers 12--21, retain 174/232 per output
sparse_s12_f0625    layers 12--21, retain 145/232 per output
sparse_s12_f0500    layers 12--21, retain 116/232 per output
sparse_s07_f0750    layers  7--21, retain 174/232 per output
sparse_s07_f0625    layers  7--21, retain 145/232 per output
sparse_s07_f0500    layers  7--21, retain 116/232 per output
top50_diagRB         layers 12--21, retain 116/232 particle terms and
                     analytically integrate the omitted diagonal Gaussian
```

At layer 21 the input retention count is still determined from 232 live
inputs; all 200 output columns remain.

`top50_diagRB` is a fixed Rao--Blackwellized arm, not another fitted
variant. It does not renormalize or zero the omitted contribution. For
each particle preactivation it forms the retained top-|W| dot plus the
analytic omitted mean, uses the closure's diagonal activation variance

```text
sigma_j^2 = sum_omitted_i W_ij^2 v_i
```

and replaces ReLU by its exact scalar Gaussian expectation

```text
E[ReLU(t + sigma G)]
  = t Phi(t/sigma) + sigma phi(t/sigma).
```

Closure means and variances receive the same sphere normalization as the
incumbent mean fold. Off-diagonal omitted covariance is deliberately not
used in this first bounded arm.

## Fixed rows

Targets remain unopened until all candidate predictions and hashes are
sealed.

```text
Full       383, 389, 397, 401
Generated  109, 113, 117, 121
```

## Gates

The control must first associate on Full0 at relative RMSE `<=2e-6` and
maximum absolute error `<=3.2e-5`.

A sparse candidate passes the accuracy gate only if, separately in Full
and Generated:

- pooled MSE ratio to `mean_s12_w216` is `<=1.10`;
- maximum row ratio is `<=1.50`.

All candidates are reported. A candidate cannot advance without an
independent lawful FlopScope count derivation establishing a material
count reduction after selection, rescaling, indexing, products, and
reductions are all priced. The dense masked Torch replay is never used as
cost evidence.

## Boundaries

The shared benchmark lock is mandatory. Capture opens only weights and
seeds. A separate scorer validates the exact prediction seal before
opening targets. No physical row, package, network, remote action, upload,
submission, or `STATUS.json` mutation is authorized.
