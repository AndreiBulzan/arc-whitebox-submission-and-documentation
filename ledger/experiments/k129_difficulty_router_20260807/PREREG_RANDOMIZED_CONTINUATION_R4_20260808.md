# Nested-basis randomized continuation — R4 preregistration

Date: 2026-08-08. This is a target-free finite-population capture followed
by one fixed post-seal score on historically opened Full100 and Generated128
development banks. It changes no estimator and performs no physical row,
FlopScope session, package, upload, submission, or remote action.

## Question

The expert proposed an unbiased continuation estimator

```text
Yhat = C + B/p * (F-C),  B ~ Bernoulli(p),
```

where `C` is an `n`-basis prefix and `F` is the full 129-basis mean. Its
ordinary unweighted MSE depends on the target-free discrepancy
`D=||F-C||^2/256`. The official challenge, however, multiplies each realized
row loss by that row's realized compute multiplier before averaging. The
cheap and continued branches therefore receive different multipliers.

Writing `a=||C-mu||^2/256`, `d=||F-C||^2/256`, and
`t=<C-mu,F-C>/256`, the exact expected adjusted row loss is

```text
L(p) = qC*a + (qF-qC)*p*a + 2*qF*t + qF*d/p.
```

The conditional optimum is

```text
p* = min(1, sqrt(qF*d / ((qF-qC)*a))).
```

Thus the official scoring rule invalidates the claim that `D` alone is
sufficient: the optimum also needs the unknown cheap raw error `a`. R4 asks
whether the expert's prefix-variance statistic plus a single training-corpus
scale nevertheless gives a useful target-blind approximation.

## Frozen data and arms

- target-free K129 per-basis final endpoints on Full100 and Generated128;
- the existing test500 research `raw_m` labels solely to freeze one global
  cheap-error scale per `n`;
- `n in {16,24,32,48}`;
- `qF=0.5133`;
- modeled cheap effective costs `{22.52,30.28,38.04,53.56}B`, converted by
  `qC=max(0.1,C/272B)`;
- the literal first `n` bases are the production-shaped prefix;
- randomized replay uses 64 universal basis permutations from fixed seed
  `2026080841` and never opens targets.

The archived full prediction is numerically the mean of all 129 endpoints;
the capture must verify maximum disagreement below `1e-12`.

## Target-free statistic and frozen policy

For prefix basis contributions `y_b`, define

```text
s2 = sum_b ||y_b-C||^2 / (256*(n-1))
Ghat = (1-n/129)/n * s2.
```

This is unbiased over a random without-replacement prefix for the expected
`D`. The frozen evaluation-time continuation probability is

```text
p_proxy = clip(
    sqrt(qF*Ghat / ((qF-qC)*abar_train)),
    0.5,
    1.0,
)
```

where `abar_train` is the test500 mean `raw_m` at the same `n`. No held
target, row ID, weight family, or post-seal parameter enters this policy.

Target-free gates:

- finite endpoint/prediction identity and statistic arrays;
- replay Pearson correlation between `log(Ghat)` and `log(D)` is reported
  for each family and `n`;
- primary signal gate: correlation `>=0.50` in both families for at least
  one `n`.

Failure of the signal gate does not prevent the fixed score; it diagnoses
why the proxy fails if score also fails.

## Fixed post-seal reports

For every family and `n`, report:

1. full-arm adjusted baseline;
2. exact expected adjusted loss of `p_proxy`;
3. 4,096 Bernoulli suite simulations, including mean and p95 score ratio;
4. a target-dependent per-row `p*` oracle with the same `p>=0.5` safety
   floor, as a capacity bound;
5. an unrestricted `p>=0.05` oracle, as a diagnostic bound;
6. a reciprocal-family constant probability obtained from aggregate
   `mean(a),mean(d)` on the other development family.

The primary policy passes only if one frozen `n` has:

- expected score improvement `>=3.5%` on each family;
- pooled expected improvement `>=5%`;
- simulated p95 score ratio `<=1.0` on each family.

If the `p>=0.5` oracle itself fails to improve both families, randomized
continuation is capacity-killed under the conservative safety floor. If the
oracle passes but `p_proxy` fails, close only this prefix-variance/global-
scale spelling. Any future continuation rule must supply genuinely new
information about `a`, not merely retune the same `Ghat`.

All adjusted values are **projection** evidence because arm multipliers are
modeled constants. Target-free replay correlations are **component**
evidence; post-seal development-bank scores are **broad statistical** plus
projection. Mini100 remains closed.
