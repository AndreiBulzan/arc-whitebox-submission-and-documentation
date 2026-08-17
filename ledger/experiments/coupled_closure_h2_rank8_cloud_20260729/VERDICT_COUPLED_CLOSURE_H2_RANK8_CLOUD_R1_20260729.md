# Coupled closure-axis rank-8 H2 cloud R1 — verdict

Date: 2026-07-29

Evidence label: **component**. The capture opened only weights from Full
`{0,1}`, Generated `{0,1}`, and the sealed target-free direct-K24 arrays.
The rank-eight predictions were frozen to an NPZ with SHA-256
`0aecb5cf6ef0a61a2a746b6f6c94cfc594cd2d5d2e30243493b140870af6c438`
before a separate scorer opened the four development targets. No FlopScope
session, physical/timed benchmark, package, upload, submission, or remote
action occurred.

## Decision

**Kill this exact rank-eight cross-basis H2 recurrence. No rank, ridge,
anchor, or schedule sweep follows.**

It fails every accuracy/stability gate by a wide margin:

| K24 final | direct H1 MSE | rank-8 H2 MSE | candidate/direct |
|---|---:|---:|---:|
| Full2 | `1.129845004e-5` | `4.896207539e-4` | `43.3352` |
| Generated2 | `1.858972301e-6` | `6.388664196e-4` | `343.6665` |

The individual row ratios were:

```text
Full       21.4257, 458.0411
Generated 243.8867, 649.7378
```

The frozen keep gate required both pooled ratios `<=0.50`, every row
`<=1.50`, and projected count `<=60B`. Only the count gate passed.

## The state was real, not inert

The fixed rank-eight axes were the top Price-closure preactivation
covariance eigenvectors at every layer. The live odd cloud was projected
onto them, pooled across all 24 bases, whitened to `I/256`, and expanded
into all 36 symmetric H2 features. One cross-basis core per output was fit
without targets and coupled before every subsequent matrix.

The construction materially increased represented even energy:

```text
one-axis layer-31 explained fraction       4.26%--5.66%
rank-eight layer-31 explained fraction     8.01%--12.17%
```

The 36-feature/core identity, Gaussian connected-quadratic variance, and
whitening identities all passed to numerical precision. Every K4/K24 state
remained finite. Thus the result is not a fit failure or numerical blowup.

It is an analytic-anchor failure under iteration. The post-seal layer
diagnostic shows a small initial worsening that compounds:

| layer | Full candidate/direct | Generated candidate/direct |
|---:|---:|---:|
| 1 | `1.077` | `1.036` |
| 4 | `1.193` | `1.324` |
| 7 | `1.589` | `1.663` |
| 11 | `3.899` | `5.005` |
| 15 | `26.105` | `45.364` |
| 31 | `43.335` | `343.666` |

The moment-matched Gaussian law of the connected quadratic is not accurate
enough to serve as a coefficient-one layerwise control. Capturing twice as
much sampled even energy does not make its finite-node quadrature error an
unbiased proxy for the live non-Gaussian error.

## Distinction from already closed work

This was not another endpoint cumulant:

- no PTCC or K3/K4 endpoint correction was applied;
- no teacher state or score-time moment was used;
- the state changed the cloud before layers `1..31`;
- covariance selected only eight axes, while the sign-bearing H2 core came
  from the live cloud.

The negative result nevertheless agrees with the connected-innovation
boundary: a perturbative Gaussian base can carry a real connected state in
the correct representation and still compound the wrong nonlinear law.
Reopening by increasing rank or retuning the anchor is not justified.

## Economics

Evidence label: **projection**, not a FlopScope receipt.

```text
K24 propagated graph projection       24.753301624B
rank-eight H2/closure ceiling         30.000000000B
total count projection               54.753301624B
```

The graph plausibly fits below `60B`; accuracy kills it. No wall, residual,
effective-compute, cap, or runnable-estimator claim follows.

## Frozen artifacts

```text
preregistration
8de240dd6766db48e056bfdfecc72583efa369673768fad45b5988374558632a

source
cbb042087a0a824d947d0857fef147a9764cb9dd92f1b3cfa0073dc960b372fc

unit receipt
42d952a53e92b7028cfe8b0950e8eb9c862e26099982ca4f3bd464d4de64d2f5

target-free K24 predictions
0aecb5cf6ef0a61a2a746b6f6c94cfc594cd2d5d2e30243493b140870af6c438

target-free manifest
66872234777c2f7ffcd01ff66a1f29e405cbddfabd328fbd2f6b57f5e83c1bca

post-seal score
7bbcb85bd46edc3f017f1347ae523042eaaa7899c2303c13be23b1f684e6dfe5
```

