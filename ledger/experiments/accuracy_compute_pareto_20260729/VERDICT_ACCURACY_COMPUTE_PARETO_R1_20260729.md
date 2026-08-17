# Accuracy × compute Pareto audit — verdict

Date: 2026-07-29

## Outcome

There is one lawful **central checkpoint projection** below `1.2e-7`:

```text
candidate       unchanged K162/m33 lambda0, literal 129:33 blend
runtime         persistent-operands R1 (prediction byte-identical)
count           159.552286 B                  component measurement
residual          0.154029 s                  component measurement
effective       174.955228 B                  projection attachment
multiplier        0.643218
```

Calibrating the paired K162/K146 raw-accuracy ratio on the two broad
development families to remote submission 320262's K146 raw MSE gives:

| transfer calibration | adjusted central | bootstrap p95 |
|---|---:|---:|
| Full100 paired ratio | `1.18827e-7` | `1.33290e-7` |
| Generated64 corrected paired ratio | `1.15505e-7` | `1.33956e-7` |

This is explicitly a **projection**, not a broad or remote K162 result.
The remote and development ratios do not use the same MLP rows. It clears
the desired central checkpoint in both calibrations, but not the conservative
p95 checkpoint.

The candidate specification contains no new fitted coefficient, target-mined
inference decision, support change, or numerical change. It is the existing
target-free S33 support and literal blend, attached to a
prediction-preserving runtime rewrite.

## Direct-bank reality

At the same component-price projection, the literal K162 frozen bank gives:

| direct development bank | raw central | adjusted central | adjusted p95 |
|---|---:|---:|---:|
| Full100 | `2.15707e-7` | `1.38747e-7` | `1.53688e-7` |
| Generated64 corrected | `2.11315e-7` | `1.35922e-7` | `1.59697e-7` |

Thus the local broad banks themselves do **not** establish `1.2e-7`.
The crossing comes entirely from applying the measured remote K146
generalization level (`2.09047e-7`) and the paired K162/K146 improvement:

```text
K162/K146 raw ratio, Full central / p95       0.88372 / 0.92318
K162/K146 raw ratio, Generated central / p95  0.85901 / 0.95382
```

The K152 line is dominated (`1.518e-7` Full, `1.466e-7` Generated central
adjusted at its projected price). The old literal-support Pareto from K162
through K226 has no two-family direct-bank crossing: its best Full adjusted
central is about `1.426e-7`, while the larger supports' Generated gains
reverse by family.

## Cheap alternatives rejected

- The combined S33 support improves Full only `0.597%` while improving
  Generated `6.986%`. At the K162 price it remains `1.379e-7` Full /
  `1.264e-7` Generated directly, and its original two-family gate is
  `kill`. Without a target-free held-row selector it is not a successor.
- The fixed one-sixth arm reblend has no supported target-free derivation and
  its paired p95 ratios exceed one in both families.
- A preregistered screen of five fixed P33/P97 mixtures (equal,
  square-root-support, two inverse-variance spellings, and Richardson)
  failed every `1.2e-7` and two-family non-regression gate. The support union
  would cost K228; the best fixed candidate still projected at least
  `1.507e-7` Full centrally.
- M214 core/fringe saves only `2.111B` statically and failed component
  association; it is not prediction-preserving.
- Natural block sparsity has at most a `~1.5%` optimistic K162 effective-work
  saving and explodes request count; it cannot fund the required direct-bank
  gap.

Therefore no current direct-scored support/allocation/mixture family crosses
`1.2e-7` without either remote transfer or a genuinely new accuracy or
compute mechanism.

## Fastest decisive gate

Do not do another support or coefficient search first. Freeze the unchanged
K162 persistent runtime, correct the stale predeclared call-count gate, and
obtain a passing exact whole receipt. Then capture a disjoint local target
bank for this exact K162 identity. That single test decides whether the
`1.155--1.188e-7` remote-calibrated central projection is real enough to
package.

The current component receipt executed a complete target-free Full0
prediction with exact count and byte-identical output, but is labeled
`component/status=fail` because the expected call count was stale. Its price
must remain a projection until the corrected receipt passes.

## Evidence

Primary receipt:
`accuracy_compute_pareto_r1_20260729.json`

Fixed-mixture falsifier:
`fixed_nested_endpoint_mixtures_r2_20260729.json`

No estimator prediction, physical row, target access, network action,
remote action, upload, or submission was performed by this audit.
