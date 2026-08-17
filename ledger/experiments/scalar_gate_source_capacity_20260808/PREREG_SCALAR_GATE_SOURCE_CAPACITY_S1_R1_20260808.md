# Scalar gate-source capacity S1/R1 preregistration

Date: 2026-08-08

Evidence scope: stratified target-free **component** capture followed by a
post-seal capacity score.  This is not an exact all-centre theorem
evaluation, a physical receipt, Mini100 gate, package, upload, or remote
result.

## Question

Does the exact independent scalar-source importance-sampling class have
enough best-case capacity at `S=4,096--5,000` source transports to retain a
material part of the Gaussian-packet gain?

For a canonical branch at activation layer `l`, define the exact ReLU secant
source

```text
b_l = relu(z_l^0 + delta_z_l) - relu(z_l^0)
      - 1{z_l^0>0} delta_z_l.
```

The source is coordinatewise nonnegative.  If `J_l` is the actual downstream
canonical Jacobian from `h_l` to `h_32`, scalar source `(l,j)` has norm
`E[b_lj] ||e_j^T J_l||`.  With the packet normalization included, let `L`
be the sum of all scalar source-vector norms and let `Delta` be their vector
sum.  The oracle lower envelope for every i.i.d. scalar-source sampler is

```text
V_min(S) = (L^2 - ||Delta||^2) / (256*S).
```

This experiment estimates `L` directly.  It does not substitute the small
16-centre full-covariance packet teacher for an all-centre truth.

## Fixed development population

Use already-open rows:

```text
Full       640, 641
Generated  88, 89
```

Use all 129 Kerdock bases and exactly four line indices selected uniformly
without replacement within every basis by frozen seed `2026083117`, for 516
stratified lines per network.  Both antipodal canonical branches are kept.

Use the sealed packet law `epsilon=0.20`, with 64 independent Gaussian-noise
replicates and the exact `+z,-z` antithetic pair on each branch.  Use 64
Rademacher output probes for downstream Jacobian row norms.

## Estimator and uncertainty

For each sampled line, branch, layer and neuron:

1. average the exact nonnegative secant source over packet replicates;
2. estimate `||e_j^T J_l||` from the mean square of 64 reverse Rademacher
   probes through the exact canonical gates;
3. sum their product over branch/layer/neuron;
4. apply `E[R] / (2*r_packet)` and the uniform line average.

Float32 secant values are required to be at least `-1e-5`; values below zero
are clamped to mathematical zero and the observed minimum is recorded.

Store line-level norm-sum contributions, not benchmark targets.  Store five
fixed variants:

- all noise and all probes;
- first and second noise halves with all probes;
- all noise with first and second probe halves.

The post-seal analysis bootstraps the four sampled lines independently
within each of the 129 bases.  Noise-half and probe-half disagreement is a
separate stability diagnostic; it is not silently folded into a spurious
exact confidence interval.

The vector sum `Delta` comes from the separately sealed exact complete-basis
packet-response capture.  The scalar-norm estimate must obey the triangle
check `L >= ||Delta||` within numerical/sampling tolerance.

## Gates

Report `S = 1,024, 2,048, 4,096, 5,000, 8,192`.

The relevant broad score gate uses the actual four-row allowance for 35%
raw reduction:

```text
V_allow = q0_raw*(1-0.35) - ideal_packet_raw.
```

Also report the older `8.68e-9` local allowance separately; do not conflate
it with the broad gate.

Survive if the central `V_min(5000)` is below the broad allowance in both
families and the pessimistic noise/probe-half variant does not exceed twice
that allowance.  A strong pass licenses a production-shaped JVP/VJP oracle.

Reject the **independent scalar-source sampler at 5,000 transports** only if
the lower bootstrap quantile and every half variant exceed the broad
allowance.  A rejection does not kill grouped, correlated, stratified, or
deterministic source estimators.  If uncertainty overlaps the gate, the
result is inconclusive and centre/noise/probe counts must be increased.

## Integrity

- Hold `runtime/.benchmark_lane.lock` during GPU capture.
- Refuse overwrite and pin source, preregistration, compact Kerdock asset,
  input archives, grouped-source capture, and output.
- The capture must not open targets.
- No Mini100, FlopScope, physical runner, package, upload, or submission is
  authorized.
