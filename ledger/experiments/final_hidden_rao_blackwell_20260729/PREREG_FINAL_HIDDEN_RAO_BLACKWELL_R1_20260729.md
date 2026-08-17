# Final-hidden Rao--Blackwell R1 preregistration

Date: 2026-07-29

Evidence scope: target-free CUDA component capture followed by a separate
post-seal score.  No FlopScope session, physical row, candidate package,
upload, submission, or remote action.

## Hypothesis

Deep hidden covariance is strongly concentrated, while a purely Gaussian
closure loses the directed non-Gaussian signal.  Keep the empirical law in
the leading hidden principal coordinates and integrate only the orthogonal
residual analytically.

For the angular final hidden cloud `H`, form its empirical mean, covariance,
and leading eigenspace `U_r`.  For each final weight column `w`,

```text
m_s(w) = w' (mean(H) + U_r U_r' (H_s - mean(H)))
v(w)   = w' (Cov(H) - U_r Lambda_r U_r') w.
```

The candidate is

```text
E[R] * mean_s GaussianReLU(m_s(w), v(w)).
```

The second fixed arm scales `v(w)` per sample by the normalized squared
orthogonal residual energy.  This retains one scalar of conditional
heteroskedasticity without fitting a coefficient or opening a target.

This is not a final-output regression, a control variate, or the earlier
input active-subspace mixture.  It directly changes the integration state:
an empirical non-Gaussian low-rank mixture plus an analytic orthogonal
conditional integral.

## Fixed screen

```text
Full rows        0, 1
Generated rows   0, 1
total K          16, 32 complete antipodal Kerdock bases
ranks            2, 4, 8, 16, 32
residual modes   constant covariance, sample-energy scaled covariance
baseline         literal final-cloud mean
```

All predictions, spectra, and target-free identities are frozen before the
separate scorer opens `official_alm` or `target_rows.npy`.

Promote only if one fixed `(rank, mode, K)` candidate satisfies on both
families:

- pooled final-layer MSE ratio to its literal same-K baseline `<= 0.50`;
- maximum individual-row MSE ratio `<= 1.25`.

The deployment count is only a projection at this stage.  A promoted
spelling must plausibly remain below `55B` counted work including its
low-K structured propagation and the final eigenspace/mixture readout.

