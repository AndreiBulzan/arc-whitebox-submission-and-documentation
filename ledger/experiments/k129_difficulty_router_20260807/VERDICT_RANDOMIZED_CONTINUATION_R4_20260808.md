# Nested-basis randomized continuation — R4 verdict

Date: 2026-08-08.

## Decision

**Reject the prefix-variance/global-scale randomized-continuation policy.**
The finite-population discrepancy statistic is valid and measurably
predictive, but the official per-row scoring rule makes it insufficient.
Do not build a continuation package from this result.

This does not eliminate the already known deterministic routing oracle. It
shows that randomized debiasing does not escape the same missing observable:
the row's unknown cheap-arm error.

Evidence labels:

- prefix replay and discrepancy diagnostics: **component**;
- Full100/Generated128 development scores: **broad statistical**;
- all adjusted scores: **projection**, because the arm multipliers are
  modeled rather than measured conditional-package receipts.

No physical row, FlopScope session, Mini100 opening, package, upload,
submission, or remote action occurred.

## The scoring correction

For

```text
Yhat = C + B/p * (F-C),  B ~ Bernoulli(p),
```

ordinary MSE is unbiased relative to `F` plus `(1-p)D/p`. That identity does
not determine the challenge score because the `B=0` and `B=1` rows receive
different compute multipliers. Under the official per-row rule, the exact
expected adjusted loss is

```text
L(p) = qC*a + (qF-qC)*p*a + 2*qF*t + qF*d/p,
```

where `a=MSE(C,truth)`, `d=MSE(F,C)`, and
`t=<C-truth,F-C>/256`. The optimizing probability depends on both `d` and
the unknown `a`:

```text
p* = min(1, sqrt(qF*d / ((qF-qC)*a))).
```

The expert's claim that only the cheap--full discrepancy is needed applies
to unweighted MSE, not to this row-weighted score.

## What passed

The proposed target-free finite-population statistic was not noise:

```text
                 Full100 log-corr   Generated128 log-corr
n=16                  0.5462              0.5688
n=24                  0.5633              0.5898
n=32                  0.5767              0.5878
n=48                  0.5913              0.6045
```

These correlations use 64 universal basis permutations. Mean `Ghat/D`
ratios for the production-shaped first prefixes were `0.912..1.014` on
Full100 and `0.956..1.047` on Generated128. The statistic therefore
estimates discrepancy substantially better than the earlier difficulty
features estimated signed routing gain.

## What failed

The frozen policy substituted `Ghat` for `d`, one test500 global raw-error
scale for `a`, and clipped `p` to `[0.5,1]`. Its expected pooled score ratios
were:

```text
n=16   1.16756
n=24   1.07455
n=32   1.05016
n=48   1.02441
```

Every choice worsened score. Process-separated results also worsened for
every `n`; at the least-bad `n=48`, expected ratios were `1.03591` on
Full100 and `1.01636` on Generated128. Their 4,096-simulation p95 ratios
were `1.06209` and `1.03647`, so the failure is not confined to an
expectation with unacceptable suite tails.

A reciprocal-family constant-probability calculation chose `p=1` at every
`n`: without row-specific cheap-error information, the best aggregate
action is simply to run the full arm.

The target-dependent `p>=0.5` oracle does have small expected capacity, but
only `2.9--4.2%` on Full100 and `3.8--5.3%` on Generated128. This is much
smaller than deterministic arm-selection capacity and often has a simulated
p95 ratio above one. It is not a breakthrough even before the unavailable
`a` predictor and literal-arm costs are considered.

## Boundary

Close:

- the claim that discrepancy magnitude alone solves routing under the
  official score;
- the prefix-variance plus global-error-scale probability rule;
- constant-probability randomized continuation;
- routine tuning of its clipping floor.

A successor would need genuinely new target-blind information about the
cheap arm's row-specific raw error. That is the same hard object that the
direct signed-gain router failed to predict. The materially different
equivariant weight encoder remains logically open, but this experiment adds
no evidence in its favor.

## Artifacts

- preregistration:
  `PREREG_RANDOMIZED_CONTINUATION_R4_20260808.md`
- target-free source:
  `run_randomized_continuation_r4_targetfree_20260808.py`
- post-seal scorer:
  `score_randomized_continuation_r4_postseal_20260808.py`
- target-free capture:
  `runtime/artifacts/k129_randomized_continuation_r4_targetfree_20260808.npz`
- target-free report:
  `runtime/artifacts/k129_randomized_continuation_r4_targetfree_20260808.json`
- post-seal receipt:
  `runtime/artifacts/k129_randomized_continuation_r4_postseal_20260808.json`

SHA-256:

```text
preregistration  feed9368c0bfa10f05b01fed935cc57831f613a6c4dc514d2477715a428558ea
targetfree src   d8164f0fad25270c88f6ccb697fde56f349cbca950c37a30802da0932507abe4
postseal src     d0d884f8ade0115bf8a06eb14fe4b6f12600bba0816e0161f015bf1f47a9030e
capture          1dfa9acca51ffaf1fc72825b9175e7b786f0268d18a149b4237a849e5cfbdd4c
capture report   3611ad30ef3f2847cd6af298f7aaef3ebf60c00b4573f7ddd2909269f5c136ea
postseal receipt 3041ec56cf0832fbad3005acd5452d35f7de95b7cbe2a7a1f34de9e1969d24c3
```
