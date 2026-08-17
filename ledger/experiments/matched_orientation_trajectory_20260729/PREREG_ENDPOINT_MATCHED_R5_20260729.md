# Endpoint-matched O1 population correction R5 preregistration

Evidence before scoring: **component**, target-free.

R4 fixes the cheapest spelling.  For the deployed O1 support `S17`, form
per-output, location/scale-normalized features from:

- the 17 O1 final signed per-basis means;
- the same-label 17 O0 signed per-basis means;
- the complete paid O0 `S109` mean;
- the selected O0/O1 mean and scale relation.

A single output-permutation-equivariant ridge map with `alpha=100` is fitted
on the frozen Full training half's O1 `S109` proxy.  It has 40 inputs and is
shared over all 256 outputs.  No L4/L8 state is retained because the R4
ablation showed no gain.

The frozen signed candidate preserves the K146 blend and changes only its O1
signed mean:

```text
mu1_hat = mu1_S17 + predicted_omission
signed_candidate
  = (129 * mu0_all + 17 * mu1_hat) / 146
```

The held exact signed-final-mean gate is fixed before target access:

- pooled candidate/baseline MSE `<= 0.85` on Full and Generated;
- row-ratio p95 `<= 1.20` on both;
- at least 60% of rows improve in both.

If it passes, the only licensed final-ReLU capture injects the predicted
radial signed correction as a common O1 preactivation shift before the
existing lambda-zero gamma readout, retaining the literal `129:17` arm
blend.  No correction scale, feature, support, family coefficient, or blend
weight may be selected from the signed or final-ReLU targets.

This does not authorize a physical row, package, upload, or submission.
