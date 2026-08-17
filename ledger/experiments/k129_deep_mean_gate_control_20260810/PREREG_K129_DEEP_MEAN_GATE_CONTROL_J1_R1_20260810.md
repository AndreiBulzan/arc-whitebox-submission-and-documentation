# Preregistration: K129 deep mean-gate control J1/R1

Date: 2026-08-10

## Hypothesis

The previously tested late-checkpoint LMCBR maps analytic checkpoint-mean
discrepancies to the endpoint through a fitted 129-basis contrast.  This test
instead uses the supplied network's deterministic mean-gate Jacobian.

For checkpoint layer `k`, let `mhat_k` be the uniform K129 post-ReLU mean on
the 200 production-screen coordinates and `mtilde_k` the sealed analytic
full-covariance mean on those same coordinates.  Embed their difference in
the full 256-coordinate layer and transport it through the actual supplied
weights:

```text
d_k = mtilde_k - mhat_k
J_k d_k = d_k W[k+1] diag(p[k+1]) ... W[31] diag(p[31]).
```

Two target-free gate spellings are frozen:

1. `analytic_cdf`: `p[l]` is the full-covariance analytic Gaussian gate
   probability at layer `l`;
2. `half`: every gate probability is exactly `0.5`.

The candidate is `max(0, R92 + beta * output_scale * J_k d_k)`.

## Frozen inputs and grid

- official-Mini100 sealed R92 predictions;
- sealed per-basis layer-23--30 checkpoint means and analytic means;
- official Mini100 weights only during the target-free build;
- layers `23..30`;
- beta grid
  `{-0.02,-0.01,-0.004,-0.002,-0.001,0,0.001,0.002,0.004,0.008,
    0.016,0.03125,0.0625,0.125,0.25,0.5,1}`.

All candidate directions and predictions must be frozen before targets open.
Select the single best cell on official public rows 0--49, then transfer it
unchanged to holdout rows 50--99.

## Gates

Promote only if the selected cell achieves:

- at least 2% raw reduction on public;
- at least 2% raw reduction on holdout;
- at least 3% pooled raw reduction;
- at least 55/100 rows improved.

Also report coefficient-one performance and target-aware scalar capacity as
diagnostics.  A failure closes this analytic-mean/mean-gate spelling, not
every possible deep-layer surrogate.

No physical run, package, upload, or submission is authorized.
