# Packet Price/Wick shared-covariance PW1/R1 preregistration

Date frozen: 2026-08-09

Evidence class: target-free **component** teacher oracle.  No benchmark
expectation, FlopScope row, package, upload, submission, or remote action.

## Prior-art boundary

Capsule preflight searched `Price`, `Hermite covariance`, `Bussgang`,
`bivariate ReLU`, `shared covariance`, `packet`, `anchored Gaussian`, and
`connected`.  The nearest results are:

- packet R6, which retained only Hermite order one plus an exact diagonal;
- C1/C2, which sampled the same first-Hermite law and failed at layer two;
- global Price order sweeps and anchored-tail Gaussian closures, whose input
  laws are broad mixtures rather than local packet conditionals;
- exact centre-specific full covariance R4, which passes but is unaffordable.

PW1 is a **materially new observable in the packet bridge**: all
off-diagonal Price/Wick orders are restored before testing whether covariance
may be averaged across packet centres.  It directly invalidates the
first-Hermite assumption killed by C2.  It is not a production proposal.

## Exact series

Maintain all sixteen frozen teacher-centre means and one shared dense
covariance `B`.  Initially `m_c=rho*u_c`, `B=tau^2 I`.  Before a ReLU,

```text
z_c = m_c @ W
S   = W.T @ B @ W
sigma_j^2 = S_jj
alpha_cj = z_cj / sigma_j.
```

For `X_cj = ReLU(z_cj + sigma_j G_j)`, probabilists' Hermite coefficients
satisfy

```text
E[X H_1] = sigma Phi(alpha)
E[X H_n] = sigma phi(alpha) H_(n-2)(-alpha), n >= 2.
```

Consequently the centre-averaged covariance is approximated exactly through
order `q` by

```text
B' = sum_(n=1)^q mean_c[g_nc g_nc.T] * R^n,
g_nc = E[X_c H_n] / sqrt(n!),
R_jk = S_jk/(sigma_j sigma_k),
```

with the diagonal replaced by the exact mean post-ReLU marginal variance.
The mean itself is the exact rectified-normal mean.  Also run an overpowered
`exact_bivariate` ceiling that computes and averages the complete bivariate
Gaussian ReLU covariance for every centre, without Hermite truncation.

## Frozen evidence and configurations

- the sealed four-network/sixteen-centre full-covariance packet teacher;
- two Full and two Generated networks and all 32 actual weight matrices;
- orders `q = 1,2,4,8,12,16` plus `exact_bivariate`;
- float64 arithmetic;
- checkpoint layers `1,2,4,8,16,24,32`;
- exact-source teacher recomputation and finite/PSD diagnostics.

Compare each candidate's centre-averaged correction to the centre-averaged
exact centre-specific full-covariance correction.  No target labels enter.

## Gates

The shared-covariance representation survives only if:

- `exact_bivariate` final centre-averaged fidelity `>=0.85`, cosine `>=0.95`,
  and each family fidelity `>=0.75`;
- order `q<=8` final fidelity `>=0.80`, cosine `>=0.90`, and each family
  fidelity `>=0.70`;
- order `q<=8` layer-eight fidelity `>=0.80`;
- all outputs are finite, covariance symmetry error `<=1e-9`, and minimum
  diagonal `>=-1e-12`.

If the exact-bivariate ceiling fails, close all shared-covariance Price/Wick
orders.  If the ceiling passes but `q<=8` fails, only a higher-order or exact
bivariate compression problem remains.  A pass licenses a broad all-centre
target-free packet imitation oracle; it is not an estimator or score claim.

