# First-principles packet-coupling R1--R3 verdict

Date: 2026-08-08

## Verdict

Kill the tested ordinary cross-centre Gaussian coupling bridges.  This
includes Euclidean response matching, the corrected odd/even covariance
matching, and exact Gaussian complete mixes from local pairs through all
33,024 lines.

The evidence is **component** on fixed Full2 plus Generated2 packet-oracle
rows.  The teacher arms use unavailable final responses and are capacity
tests, not estimators.  No physical row, FlopScope session, Mini100 access,
upload, submission, or remote action occurred.

## R1: ordinary response matching

Four held common treatments produced final-response signatures.  Global
matching cut the in-signature pair distance from about `251.0` for random
pairs to `120.7`, but the improvement did not transfer to fresh noises:

```text
target-free sampling MSE
independent                                  1.768377e-7
teacher basis                               1.897012e-7
teacher global                              2.183056e-7

post-seal ideal-gain retention
teacher global                                -0.3340
```

## R2: corrected covariance matching

R1 exposed a sign error in the intuitive matching objective.  If a response
is decomposed into even and odd noise components, antithetic coupling adds
the even covariance and subtracts the odd covariance.  R2 therefore froze

```text
cost(i,j) = <E_i,E_j> - abs(<O_i,O_j>)
```

using eight held common treatments and a 32-output projection.  The teacher
found strongly negative in-sample costs, but again failed on fresh noises:

```text
target-free sampling MSE
independent                                  1.679102e-7
covariance teacher basis                     1.897525e-7
covariance teacher global                    2.179131e-7

post-seal ideal-gain retention
covariance teacher global                      -0.4096
```

Thus the failure is not merely that even responses were matched with the
wrong sign.  The cross-centre treatment covariance itself is unstable across
held noise directions.

## R3: exact Gaussian complete mixes

R3 removed prediction and matching.  For group size `k`, iid Gaussian rows
were transformed as

```text
Z_i = sqrt(k/(k-1)) * (A_i - group_mean(A)).
```

Every line retains an exact `N(0,I)` marginal and every group has exactly zero
noise sum.  Group sizes 2, 8, 256, and 33,024 produced no useful reduction:

```text
target-free sampling MSE
independent                                  1.471779e-7
mix 2                                        1.595842e-7
mix 8                                        1.540619e-7
mix 256                                      1.473765e-7
global mix                                   1.486680e-7
```

The global and basis-size mixes are essentially identical to independence;
the smaller mixes are worse.  Hence there is no material centre-common
linear packet response for zero-sum Gaussian dependence to cancel.

## Interpretation

The ideal packet target (about 54% raw headroom), finite `m=8` pool, and
actual-output one-frame balancing oracle remain valid.  What fails is the
ordinary route from that capacity to a target-blind one-frame law.  Packet
response functions are high-dimensional across both centre and noise:

- low-order common dependence does not reduce variance;
- response geometry learned on 4--8 treatments does not generalize;
- even a globally zero-sum exact Gaussian coupling is neutral.

Reopen packet smoothing only for a construction that controls the realized
global output discrepancy itself, or for a theorem-backed high-chaos product
cubature materially different from Gaussian complete mixes, Kerdock-on-
Kerdock noise, and sparse Poisson probing.

## Evidence

- `runtime/artifacts/matched_packet_coupling_r1_postseal_20260808.json`
- `runtime/artifacts/covariance_matched_packet_coupling_r2_postseal_20260808.json`
- `runtime/artifacts/global_complete_mix_packet_r3_postseal_20260808.json`
