# Packet Price/Wick broad PW2/R1 preregistration

Date frozen: 2026-08-09

Evidence class: target-free **component** oracle on the sealed Full8 and
Generated8 packet population.

## Blocking question and prior boundary

PW1 proved that shared covariance is a 97.1%-fidelity local representation
and that the Price series is converged by order two.  The prior complete-K129
R6 order-one construction nevertheless had final fidelity `-65.9`, exposing
a global signed-cancellation failure invisible locally.

PW2 changes exactly the assumption not tested by R6: it restores the first
higher off-diagonal Price/Wick terms.  Run orders `q=2` and `q=4`; order four
is the convergence checksum.  No support, shrinkage, kernel, rank, target, or
learned calibration is introduced.  This is the final test of the shared
covariance Price family.

## Frozen recurrence

Use all 66,048 canonical oriented K129 centres and one shared `256 x 256`
covariance per network.  The exact rectified-normal mean and diagonal
variance are used at every layer.  For nonzero margins, form the analytic
probabilists'-Hermite coefficient matrices and update

```text
B' = sum_(n=1)^q (G_n.T @ G_n / N) * R^n,
```

where `R` is the shared preactivation correlation matrix.  Replace the
diagonal by the exact centre-averaged post variance.  This is identical to R6
at order one and adds the exact H2/H3/H4 covariance tails at order four.

## Evidence, metrics and gates

- sealed Full8 plus Generated8 weights and independent 64-replicate Gaussian
  packet population;
- orders `q=2,4` only, float32 GPU products with TF32 disabled and float64
  scoring;
- checkpoint layers `1,2,4,8,16,24,32`;
- exact Q0 association, covariance symmetry/diagonal checks;
- packet-mean Monte Carlo noise subtracted from correction and residual MSE.

A pass requires either order to have final pooled fidelity `>=0.70`, cosine
`>=0.85`, Full/Generated fidelities `>=0.60` each, and layer-eight fidelity
`>=0.60`.  All outputs must be finite and Q0 association `<=2e-6`.

If both fail and order four changes order two final predictions by less than
10% of packet-correction RMS, close all higher shared-covariance Price orders:
PW1 already establishes series convergence.  A pass licenses only a
sketched coefficient-Gram oracle and post-seal target association, not a
production or score claim.

