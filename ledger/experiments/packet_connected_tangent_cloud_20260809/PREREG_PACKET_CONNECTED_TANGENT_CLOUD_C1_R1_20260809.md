# Packet connected-tangent cloud C1/R1 preregistration

Date frozen: 2026-08-09

Evidence class: target-free **component** oracle.  No benchmark expectation,
FlopScope prediction, physical row, package, upload, submission, or remote
action is in scope.

## Prior-art and target-ceiling preflight

Capsule searches covered `packet`, `variance field`, `covfactor`, `shared
covariance`, `moment pilot`, `boundary`, `Born`, `Price`, `Wick`, and
`connected`.  The nearest controlled negatives are:

- block/diagonal conditional covariance (`R3`);
- centrewise stochastic covariance factors of ranks 1--8 (`R5`);
- one centre-averaged dense covariance (`R6`);
- canonical/analytic total-variance subtraction (`T1/T2`);
- one- and two-treatment packet second-moment pilots (`P1/P2`);
- fixed full-covariance anchor schedules through sixteen anchors (`A2`);
- canonical-Jacobian first-Born boundary transport.

The new observable is **materially new in the capsule**.  It never estimates a
total second moment and subtracts the squared conditional mean.  It estimates
the conditional covariance directly with an ensemble tangent/Wick cloud.  It
also does not collapse the centre--covariance dependence to one dense matrix:
each pilot centre carries its own tangent realization through its own soft
ReLU gates, and only the final marginal variance is averaged.  Finally, the
resulting shared variance is used in an exact nonlinear Gaussian-ReLU mean
rollout, so boundary births interact nonperturbatively rather than through a
frozen canonical Jacobian.

The sealed true-Gaussian-packet oracle reduces pooled raw MSE by about 53.6%.
That capacity is sufficient to cross both `1.10e-7` and `8e-8` after a modest
pilot overhead if this bridge retains at least 70% of the packet correction.
Failure of the target-free correction-fidelity gate stops the mechanism before
any production or score claim.

## Frozen construction

For a fixed, target-blind nested set of `k` complete Kerdock bases, use all
`512*k` oriented centres.  For each centre maintain a conditional mean `m_c`
and `r` mean-zero tangent probes `T_ca`.  Initially

```text
m_c = rho * u_c
T_ca = tau * epsilon_ca,  epsilon_ca in {-1,+1}^256.
```

At each supplied layer `W`, compute

```text
z_c  = m_c @ W
R_ca = T_ca @ W
v_j  = mean_(c,a) R_caj^2
```

and the exact univariate Gaussian ReLU moments for `N(z_cj,v_j)`:

```text
m'_cj = E ReLU(N(z_cj,v_j))
p_cj  = Phi(z_cj / sqrt(v_j))
s2_cj = Var ReLU(N(z_cj,v_j)).
```

The first-Hermite term is transported centrewise, and the orthogonal ReLU
birth is injected with fresh target-free signs:

```text
T'_ca = p_c * R_ca
      + sqrt(max(s2_c - p_c^2 v,0)) * epsilon'_ca.
```

Across the `(centre,probe)` ensemble, centre every tangent coordinate and
rescale it to the exact aggregate `mean_c s2_c`.  This deterministic
square-root balancing removes finite-ensemble mean/scale drift while leaving
the centre-specific gate multiplication and cross-neuron Wick covariance
sample intact.

Store the resulting 32 shared variance vectors.  Separately propagate all
66,048 canonical oriented K129 centres using those variances and the exact
Gaussian-ReLU mean at every layer.  This full conditional-mean cloud is the
candidate.  It uses no packet outputs or expectation targets.

## Frozen configurations

- complete-basis supports `k = 1, 2, 4, 8`;
- tangent ranks `r = 1, 2, 4`;
- four fixed independent nested support/noise replicates;
- all sealed Full8 and Generated8 network rows from the 64-replicate packet
  population;
- float32 matrix products, TF32 disabled, float64 scoring;
- emitted layers `1,2,4,8,16,24,32`.

All support permutations and random signs are fixed from the source seed.
No configuration may be selected using packet truth while capture is being
created; every configuration is written and reported.

## Metrics and gates

Compare the deterministic candidate to the independent sealed packet
replicate mean.  Subtract the packet-mean Monte Carlo variance from both
correction energy and residual MSE.  For every configuration and replicate,
report pooled, Full and Generated correction fidelity and cosine.

A primary pass requires some `k <= 4`, `r <= 2` configuration with:

- median final pooled correction fidelity `>= 0.70`;
- median final pooled cosine `>= 0.85`;
- median Full and Generated fidelity `>= 0.60` each;
- at least `3/4` replicates with fidelity `>= 0.50`;
- median layer-8 pooled fidelity `>= 0.60`;
- exact Q0 association within `2e-6` and all values finite.

A fallback pass permits `k <= 8`, `r <= 4` under the same gates.  Passing
licenses a post-seal target association and a meter-aware R90 integration
design only.  Failure kills this connected-tangent/shared-marginal bridge,
not the exact full-covariance packet target or arbitrary higher-order
connected contractions.
