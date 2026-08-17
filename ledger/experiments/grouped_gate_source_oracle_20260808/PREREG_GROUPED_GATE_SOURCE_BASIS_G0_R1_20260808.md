# Grouped gate-source basis G0/R1 preregistration

Date: 2026-08-08

Evidence scope: target-free packet **component** capture and separate
post-seal development scoring.  This is an exact-grouping capacity oracle,
not a JVP/VJP estimator, physical receipt, Mini100 gate, package, upload, or
remote result.

## Question

Before scalarizing the exact packet Duhamel source into hundreds of millions
of neuron sources, does grouping the correction into complete Kerdock-basis
vector atoms retain enough cancellation for a `10--12B` boundary-source
implementation to be plausible?

For centre `i`, the exact conditional packet correction satisfies

```text
c[L,i] = sum_l T[L<-l,i] b[l,i].
```

Any partition of these terms is an exact regrouping.  The present oracle uses
one final vector atom per complete real-MUB basis.  It deliberately preserves
the orthogonal cancellation destroyed by the already-failed linewise Poisson
multiplex and ordinary packet couplings.

## Prior-art preflight

Capsule searches covered `Duhamel`, `gate source`, `boundary source`,
`facet`, `JVP`, `VJP`, `Price/Wick`, `connected source`, and importance
sampling.

Nearest evidence:

- the full packet target reduces raw MSE by about 53.6%;
- full per-centre covariance reproduces local packet means on a 16-centre
  micro-oracle but is not globally available;
- sparse linewise Poisson multiplexing fails because it destroys complete
  orthogonal/Latin cancellation;
- ordinary couplings and low-dimensional state summaries fail;
- no existing capture provides exact `b[l,i]` for all centres.

This experiment is a `materially new observable/grouping`: it estimates the
true conditional packet response of every complete basis directly and tests
finite-population basis-vector sampling.  It does not call the 16-centre
full-covariance micro-oracle exact or extrapolate it globally.

## Fixed rows and packet law

Use already-open rows:

```text
Full       640, 641
Generated  88, 89
```

Use the sealed packet parameters from the R2 truth oracle:

```text
epsilon = 0.20
rho     = cos(epsilon)
tau     = sin(epsilon) / sqrt(256)
```

For each of the 129 bases, retain all 256 unoriented lines and both
antipodal branches.  For every replicate use paired noises `+z,-z` at each
line, yielding four packet trajectories per line.  The canonical anchor is
`rho*u`; positive homogeneity permits reuse of the canonical path but the
source identity is defined relative to this scaled anchor, not unscaled
`u`.

Capture at least 32 independent packet replicates in the main run.  Store the
final pair-averaged packet response separately for every basis and replicate,
plus canonical basis responses.  No expectation targets are opened.

## Exact normalization and atoms

Let `r_packet = E||rho U + tau Z||`.  For basis `a`, define the unnormalized
conditional source response

```text
c_a = E_packet[F32 | basis a] - rho * Q_a F32.
```

The stochastic portion of the Gaussian-output correction is represented by

```text
Y_a = (E[R] / r_packet) * c_a / 129.
```

The remaining deterministic normalization term

```text
E[R] * (rho / r_packet - 1) * Q_D F32
```

is added exactly and is not charged to sampling variance.

The 129 atoms sum to the ideal packet correction.  This identity must agree
with the independently accumulated global packet mean within Monte Carlo
uncertainty.

## Capacity calculations

For i.i.d. basis sampling with probabilities `p_a`, the per-output added MSE
of `S` sampled bases is

```text
[sum_a ||Y_a||^2 / p_a - ||sum_a Y_a||^2] / (256*S).
```

Report:

- uniform i.i.d. basis sampling;
- oracle `p_a proportional ||Y_a||` capacity;
- uniform sampling without replacement using the exact finite-population
  covariance;
- deterministic top-head plus stochastic-tail diagnostics, clearly marked
  unavailable/oracle.

Evaluate `S = 1,2,4,8,16,32`.  Compute both central estimates and replicate
bootstrap intervals.  Split replicates in half to measure whether the atom
norm ranking and predicted capacity are stable out of sample.

The post-seal scorer must calculate expected final raw MSE directly as

```text
ideal packet raw MSE + added sampling MSE,
```

and report retained packet gain.  Do not use the four-row coupling-specific
`8.68e-9` allowance as a universal threshold.

## Cost normalization

The later production concept would propagate full-vector boundary corrections
for all antipodal centres in selected complete bases.  Its cost must be
measured separately.  For orientation only, one JVP plus one full-vector
source recurrence over one complete antipodal basis is approximately two
forward equivalents over 512 rows, or about `2.15B` conventional arithmetic.

Thus `S=4` bases is the most relevant initial `~8.6B` capacity point.  This
is a projection, not a receipt.

## Gates and careful stopping

Survival gate:

```text
S <= 4 basis atoms forecasts >=35% raw reduction, or
S <= 8 forecasts >=35% reduction with a credible <=12B implementation path.
```

Cross-family requirement: positive central reduction in both families and
no replicate-half sign reversal.

Failure of complete-basis sampling does **not** kill layer-vector,
centre-vector, stratified, correlated, or deterministic collective-source
estimators.  It kills only this complete-basis i.i.d./without-replacement
grouping at the measured budget.  Failure of the packet Monte Carlo
uncertainty check is inconclusive and requires more replicates.

## Integrity

- Hold `runtime/.benchmark_lane.lock` during GPU capture.
- Refuse overwrite and checkpoint replicates durably.
- Pin source, preregistration, compact Kerdock asset, input archives, and
  outputs.
- No Mini100, target access during capture, FlopScope, physical runner,
  package, upload, or submission is authorized.

