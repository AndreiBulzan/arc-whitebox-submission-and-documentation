# Preregistration: shared dense gate-Gram packet closure R6

Date frozen: 2026-08-07

## Blocking question

Can one full-rank `256x256` covariance shared across all 66,048 K129 packet
centres reproduce at least 70% of the measured Gaussian-packet gain through
the exact aggregate first-Hermite gate-Gram recurrence?

This is the independent secondary mechanism in the external review.  It is
not a packet-candidate selector and is unaffected by the R2/R3 Nyström
selector kills.

Passing licenses only a row-sketched gate-Gram compression oracle and a
disjoint numerical test.  It does not license estimator implementation,
compute or remote-score projection, packaging, upload, or submission.

## Prior-art boundary

The block/diagonal packet closure R3 kept centre-specific scalar or grouped
variances and discarded every cross-neuron covariance.  It failed at layer
2.  Stochastic covariance-factor R5 kept centre-specific rank `1,2,4,8`
factors and also failed.  Neither tested a full-rank dense covariance shared
across centres.

The R4 micro-oracle showed that centre-specific full covariance is nearly
exact but unaffordable.  The new representation changes the compressed axis:
it preserves all neuron-pair directions in one dense matrix and averages
only over packet centres.  This is a materially new factorization.

Outcome: **materially new collective full-rank state**.

## Frozen recurrence

Let `N=66,048`.  Maintain all centre means `mu_c` and one shared covariance
`B`.  Initialize

```text
mu_c = rho * u_c
B    = tau^2 * I.
```

For each supplied layer matrix `W`, using the capsule's row-vector
convention:

```text
m_c = mu_c @ W
S   = W.T @ B @ W
q_j = S[j,j]
a_cj = m_cj / sqrt(q_j)
p_cj = Phi(a_cj)
```

Compute the exact univariate Gaussian ReLU mean and variance for every
centre/neuron.  Define

```text
H = mean_c p_c p_c.T
r_cj = Var(ReLU(N(m_cj,q_j))) - p_cj^2 q_j
B_next = S * H + diag(mean_c r_c).
```

Here `*` is the Schur product.  This is exactly the aggregate of the first
Hermite/Bussgang covariance term under the shared-covariance assumption,
with every marginal diagonal repaired exactly.  It retains full covariance
rank; no factor truncation, group partition, learned parameter, or target
enters.

R6 uses the exact `H=P.T@P/N`.  Its cost is oracle-only and not a production
claim.  If it passes, later work may approximate `H` with a frozen row
CountSketch; that approximation is out of scope here.

## Frozen rows and packet reference

Use the same sealed packet rows and constants:

```text
Full1000:      640..647
Generated128:  88..95
centres:        all 129 Kerdock bases and antipodes
epsilon:        0.20
rho:            cos(0.20)
tau:            sin(0.20)/sqrt(256)
layers emitted: 1,2,4,8,16,24,32
```

The capture may read weights and the sealed 64-replicate target-free packet
capture.  It must write all predictions before targets are opened.  Timing
is diagnostic only; no FlopScope or physical row is in scope.

## Target-free gates

At each emitted layer define packet-correction fidelity

```text
1 - MSE(shared_closure - packet_reference)
    / MSE(q0 - packet_reference).
```

The mechanism passes the target-free rung only if:

- final pooled correction fidelity `>=0.70`;
- final Full and Generated fidelity each `>=0.60`;
- final correction cosine `>=0.85`;
- layer-8 pooled fidelity `>=0.70`;
- all outputs are finite and q0 association is exact within `2e-6`.

Failure kills one-shared-covariance K=1 before target scoring.  Passing
permits post-seal target association with the research gate of pooled ideal
packet gain retention `>=0.70`, family retention `>=0.60`, and at least
`12/16` rows improved versus q0.

Because these targets are already open from earlier packet work, any
post-seal result is **broad statistical exploratory on a reused bank**.
Target-free packet imitation is **component** evidence.

