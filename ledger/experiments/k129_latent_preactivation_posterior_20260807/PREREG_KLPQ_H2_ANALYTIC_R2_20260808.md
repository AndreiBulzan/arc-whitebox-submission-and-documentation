# Chaos-controlled KLPQ H2 analytic oracle — R2 preregistration

Date: 2026-08-08. This is a target-free component oracle. It changes no
production estimator, performs no physical FlopScope run, and performs no
package, upload, submission, or remote action. Expectation targets remain
closed until the target-free capture and its hashes have been written.

## Question and frozen order

The exact five-projector inverse from R1 is already algebraically green. The
next question is whether the constant, first, and second Gaussian-chaos terms
of the *conditioned latent-preactivation* ReLU field carry a useful signed
spherical-integral correction.

The order is frozen:

1. compute the spherical kernel mean and kernel-convolution eigenvalues;
2. capture canonical final preactivations and the analytic H2 predictions on
   the fixed rows below without opening targets;
3. seal the capture, report, source, and preregistration hashes;
4. run one separate post-seal scorer;
5. only then decide whether a dense fourth-and-higher-chaos query oracle is
   licensed.

LOBO is deferred. An omitted Kerdock basis excites only two special sectors
and is not the decisive generic-continuation geometry.

## Prior-art preflight (blocking)

Queries searched in the capsule: `H2`, `Hermite-2`, `second chaos`, `Stein`,
`zonal H2`, `q-disagreement H2`, `tree chaos`, `latent preactivation`,
`posterior ReLU`, `kernel convolution`, `association projector`, and
`Bayesian quadrature`.

Nearest controlled negatives are:

- `k129_stein_output_calibration_20260730`: a second-chaos query over the
  random *final weight row*, using an approximate layer-30 radial matrix;
- `K162_QDISAGREEMENT_H2_CROSSFIT_VERDICT_20260727.md`: a fitted quadratic
  arm-disagreement scalar;
- `coupled_zonal_h2_cloud_20260729`: a one-axis layerwise spherical H2
  surrogate which captured about five percent of late even-trace energy;
- `adjoint_tree_chaos_20260729`: local Gaussian mean/covariance diagrams for
  an unobserved connected K4 query.

Outcome: **materially new observable**. R2 conditions the normalized latent
GP on every one of the 66,048 signed final-preactivation observations, keeps
all five full-rank Kerdock sectors exactly, and integrates the resulting
posterior sector energies. It neither estimates the layer-30 radial matrix,
fits a target coefficient, uses one zonal axis, nor recursively closes an
unobserved tensor. Those are the assumptions invalidated by the new
observable.

## Exact controlled integral

Let `K` be the normalized depth-31 preactivation kernel on the signed K129
nodes, `z_j` the physical unit-sphere final preactivation for output `j`, and
`alpha_j = K^-1 z_j`. Define

```text
kbar = integral k(x,u) d sigma(u)
H(x,y) = integral k(x,u) k(y,u) d sigma(u)
```

For latent variance amplitude `q_j`, the physical H2-controlled spherical
mean is

```text
I_H2,j = sqrt(q_j) a0
       + a1 kbar^T alpha_j
       + a2 [ alpha_j^T H alpha_j / sqrt(q_j)
              - sqrt(q_j) trace(K^-1 H) ]

a0 = 1/sqrt(2*pi)
a1 = 1/2
a2 = 1/(2*sqrt(2*pi)).
```

`H` is isotropic and shares K129's five projectors. If `lambda_r`, `h_r`,
`m_r`, and `E_r,j = ||P_r z_j||^2` are respectively the kernel eigenvalue,
convolution eigenvalue, multiplicity, and observed sector energy, then

```text
alpha_j^T H alpha_j = sum_r h_r E_r,j / lambda_r^2
trace(K^-1 H)       = sum_r m_r h_r / lambda_r.
```

The Gaussian input prediction is `E[chi_256] * I_H2,j`. The canonical dense
K129 comparator is the same radial factor times the equal-weight mean of the
observed final ReLU values.

The scalar identity

```text
Var(ReLU(G) after removing H1,H2) = (pi-3)/(4*pi)
```

is checked statically. Its `3.3058%` share of scalar ReLU variance is not an
accuracy claim or an integration-error bound.

## Frozen convolution audit

For `x dot y = t`, write a uniform sphere coordinate as

```text
s = x dot u
y dot u = t*s + sqrt(1-t^2)*sqrt(1-s^2)*v.
```

Use tensor Gauss-Jacobi quadrature with exponents `(d-3)/2` for `s` and
`(d-4)/2` for `v`. Orders `{32,48,64,96}` are reported; order 96 is primary.
Compute `kbar` and `H(t)` at `t in {1,-1,0,1/16,-1/16}`, then obtain the five
`h_r` with the same association formulas as R1.

Static gates:

- depth-31 kernel values and `lambda_r` reproduce R1;
- order-64 versus order-96 `kbar` differs by at most `2e-11`;
- every order-64 versus order-96 convolution-sector eigenvalue differs by
  at most `2e-9`;
- all primary `h_r >= -1e-10`, and the analytic trace is finite;
- sector energies reconstruct `sum_i z_ij^2` to relative error `<=2e-10`;
- physical and standardized implementations of the H2 formula agree within
  `2e-11` relative/absolute tolerance on deterministic synthetic inputs.

Failure is an implementation/numerical error and opens no target access.

## Frozen rows, amplitudes, and outputs

- Full rows `640..647`.
- Generated rows `88..95`.
- The unchanged canonical float32 K129 nodes are propagated through all 32
  supplied weight matrices; the field immediately before the final ReLU is
  retained.
- Variants are `theory`, `energy`, and `ml`, exactly as in R1.
- `theory q=2/256`.
- `energy q=mean(z^2)` per output.
- `ml q=z^T K^-1 z / 66,048` per output, clipped only to
  `[0.25,4]*(2/256)`.
- `ml` is the primary fixed variant. Diagnostics cannot replace it after
  targets are opened.

The target-free capture stores the canonical K129 prediction, all three H2
predictions, amplitudes, five sector energies, analytic terms, numerical
checks, and row identities. It stores no target-derived quantity.

## Post-seal gates and target ceiling

The comparator is the literal dense canonical K129 prediction from the same
captured field, not a borrowed R87 prediction. The post-seal scorer reports
pooled and process-separated final-layer raw MSE, per-row ratios, fraction
improved, correction/residual cosine, and maximum row ratio.

Classification for the primary `ml` arm:

- **strong pass:** pooled raw-MSE reduction `>=20%`, reduction `>=10%` on
  both Full8 and Generated8, and at least 12 of 16 rows improve;
- **partial/control-only:** pooled reduction in `[10%,20%)`, neither family
  worsens by more than 5%, and correction/residual cosine is positive;
- **H2-only rejection:** otherwise. This rejects only the analytic H2-only
  estimator; it does not by itself reject the fourth-and-higher-chaos KLPQ
  posterior integral.

Target ceiling: against the public K129 raw `2.2274e-7`, a 20% raw reduction
at less than `0.1B` projected added arithmetic would conditionally place the
adjusted score near `9.1e-8`. This is a **projection**, not a receipt. The
four-frame and packet oracles demonstrate more than enough mathematical
headroom for the threshold, but do not predict this estimator's sign.

No dense query, pilot calibration, production edit, package, upload, or
submission is licensed by this preregistration alone.

## Evidence and operations

The result is `component` evidence on fixed oracle rows. GPU capture holds
`runtime/.benchmark_lane.lock`; wall time is diagnostic. There is no
FlopScope or effective-compute receipt. All new source remains in this
directory and all new physical artifacts remain under `runtime/artifacts/`.
