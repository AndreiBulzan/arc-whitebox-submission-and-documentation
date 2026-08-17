# K129 hidden-mean student R1 preregistration

Date: 2026-08-01

Evidence scope: **component** only.  This experiment performs an ordinary
target-free CUDA replay on the already materialised sparse 16-MLP public
pilot.  It authorizes no FlopScope row, estimator integration, package,
upload, submission, or remote claim.

## Hypothesis

For the final layer,

```text
E[ReLU(z31)] = 0.5 E[z31] + 0.5 E[|z31|]
E[z31]       = E[h30] W31.
```

The production cloud already creates 129 per-basis estimates of `E[h30]`.
Capture those values without opening targets.  After sealing, use whole-MLP
fourfold cross-fitting to ask whether their normalized, basis-indexed pattern
predicts the high-sample hidden-mean residual.  The downstream correction is
the exact signed-mean map `0.5 * delta_h30 @ W31`; it does not replace or
model the absolute-value term.

This is distinct from the killed final-endpoint decoders and fixed supervised
basis weights: the supervised object is the upstream hidden mean, and the
coefficient is allowed to depend on the realised per-basis hidden state.

## Stop-fast gate

Promote to a broader target-free capture only if the sparse diagnostic has:

```text
cross-fitted hidden-mean MSE ratio       <= 0.80
cross-fitted final-output MSE ratio      <= 0.92
MLPs improved in final output            >= 10 / 16
```

Anything weaker is killed without estimator work or physical timing.
