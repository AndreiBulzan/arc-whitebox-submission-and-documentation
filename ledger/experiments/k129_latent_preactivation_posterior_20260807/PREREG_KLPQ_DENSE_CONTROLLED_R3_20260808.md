# Dense chaos-controlled KLPQ posterior integral — R3 preregistration

Date: 2026-08-08. This is a target-free component oracle followed by one
fixed post-seal score. It changes no production estimator and performs no
physical FlopScope run, package, upload, submission, or remote action.

## Controlling question

R2 proved that the analytic H0/H1/H2 posterior integral is not an estimator:
its primary raw MSE was `1.9333e-2` versus canonical `3.4622e-7`. The missing
fourth-and-higher posterior chaoses must cancel the H2 integral almost
completely.

R3 performs the remaining decisive pure-K31 test. For each generic query
direction it evaluates the full conditioned rectified-normal mean

```text
eta(u) = sqrt(q v(u)) phi(m(u)/sqrt(q v(u)))
       + m(u) Phi(m(u)/sqrt(q v(u)))
```

and integrates `eta-c` numerically while adding the exact R2 integral
`I(c)`. It also records the uncontrolled direct query mean of `eta`.

If this dense full posterior integral fails, H2 clipping or refitting is not
a remedy: pure depth-31 isotropic KLPQ continuation is rejected.

## Prior-art and reopen boundary

Queries searched: `latent GP`, `posterior ReLU`, `rectified normal`,
`Bayesian quadrature`, `final hidden Rao-Blackwell`, `NNGP selector`,
`posterior query`, `generic rotation`, `control functional`, and `H2`.

Nearest negatives are activation-GP Bayesian quadrature, universal and
first-layer NNGP packet selectors, final-hidden Gaussian closure, and the R2
H2-only rejection. Outcome: **materially new observable**. R3 retains every
signed final preactivation, conditions the latent field exactly through the
five full-rank Kerdock projectors, and evaluates the nonlinear posterior
ReLU mean at generic directions. It does not infer packet candidate labels,
replace the deep trajectory law recursively, or model observed activations
as Gaussian.

Target ceiling: canonical-to-four-frame and packet oracles demonstrate more
than 35% raw headroom. This passes the capacity preflight but does not predict
the K31 continuation sign.

## Frozen observations and amplitudes

- Full rows `640..647`; Generated rows `88..95`.
- The same repaired association geometry and unchanged physical canonical
  float32 nodes as R1/R2.
- The same depth-31 normalized kernel, five eigenvalues, and exact inverse.
- Amplitudes `theory`, `energy`, and `ml` exactly as R2; `ml` remains primary.
- No clipping of predictions or target-fit shrinkage is permitted.

## Frozen dense query rules

Construct two independent universal rules. For each rule, generate four
Haar orthogonal `256x256` frames from fixed NumPy RNG seeds `2026080831` and
`2026080832`; include every row and its antipode. Thus each rule has 2,048
directions and is exactly antipodal with exact empirical second moment
`I/256`. The final dense estimate averages the two controlled rule outputs,
using 4,096 total query directions.

Query rules are identical for every network and use no weights, outputs,
targets, IDs, or row-specific selection. Record their array hashes, mean
norms, second-moment errors, and cross-rule controlled prediction
disagreement.

All posterior operations use float64. Query rows are streamed in batches of
64. For every batch:

1. evaluate `k(q,D)` on all 66,048 signed K129 nodes;
2. apply the exact five-projector inverse to obtain
   `v(q)=1-k(q,D)K^-1k(D,q)`;
3. contract the same kernel row with `K^-1 z_j` for every output;
4. evaluate `eta`, the physical H2 control `c`, and `eta-c`.

## Target-free gates

- query mean norm and second-moment maximum error `<=2e-13`;
- all posterior variances lie in `[-1e-10,1+1e-10]`;
- each rule's mean posterior variance is within `5e-4` of the analytic
  `1-tr(K^-1 H)`;
- observed-node interpolation remains below `1e-8`;
- no nonfinite prediction;
- the two-rule controlled discrepancy
  `mean(||Q1-Q2||^2)/(2*256)` is reported per row and pooled. A pooled value
  `<=3.3411e-8` is the preregistered numerical-query sufficiency gate for
  retaining a possible 20% raw improvement.

Failure of a variance or interpolation gate is an implementation error.
Failure only of the query-disagreement gate means this 4,096-query oracle is
not numerically decisive and licenses no accuracy claim.

## Post-seal accuracy gates

The primary prediction is the average of the two controlled `ml` rules.
The scorer reports all amplitude variants, each individual rule, the
two-rule average, the direct-uncontrolled query estimate, process-separated
results, row ratios, fraction improved, and correction/needed cosine.

- **strong pure-K31 pass:** pooled raw reduction `>=35%`, each family
  reduction `>=25%`, at least 12 of 16 rows improve, correction/needed cosine
  is positive, and the target-free query-disagreement gate passes;
- **calibration-only shape pass:** pooled reduction in `[15%,35%)`, neither
  family worsens, cosine `>=0.20`, and the query-disagreement gate passes;
- **reject pure K31 KLPQ:** otherwise.

A strong pass licenses query compression experiments, not production. A
calibration-only pass licenses only generic-pilot kernel calibration. A
rejection closes H2 repair, pure-K31 dense integration, LOBO, and query-rule
optimization. It does not test a future pilot-calibrated finite-network
kernel, but absent positive continuation shape no pilot work is warranted.

## Evidence and operations

The result is `component` evidence. GPU work holds
`runtime/.benchmark_lane.lock`; timing is diagnostic. New source is stored
here and artifacts under `runtime/artifacts/`. Targets remain closed until
the capture report, array, source, preregistration, and query hashes are
sealed.
