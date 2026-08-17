# Chaos-controlled KLPQ H2 analytic oracle — R2 verdict

Date: 2026-08-08. Evidence label: **component** on fixed Full8 and
Generated8 rows. No FlopScope session, physical row, production edit,
package, upload, submission, or remote action occurred.

## Decision

**Reject the H2-only estimator decisively.** Preserve the exact H2 integral
only as a numerical control inside one separately preregistered full
posterior-ReLU query oracle.

The primary `ml` amplitude result was:

```text
canonical K129 raw MSE             3.4621648278e-7
H2-only raw MSE                    1.9333418165e-2
candidate / canonical ratio        55,841.9923
rows improved                      0 / 16
H2 correction RMS                  1.3905798607e-1
actual missing-signal RMS          5.8840163390e-4
correction / needed cosine         0.0247583
negative predicted coordinates     1,092 / 4,096
```

Both process-separated families failed by more than four orders of
magnitude. The diagnostic `theory` and `energy` amplitudes also failed every
row, so the result is not an amplitude near miss.

## What remains mathematically valid

The convolution audit passed independently:

```text
kernel mean                                      0.9734283554575063
trace(K^-1 H)                                    0.9769344588374572
integrated normalized posterior variance         0.0230655411625428
order-64 / order-96 sector-eigen max delta       2.1828e-11
observed-node interpolation max error             1.9160e-11
sector-energy reconstruction max relative error  1.7001e-14
```

The scalar Hermite identity also remains exact: H4-and-higher terms contain
`3.3057793%` of an unconditional scalar ReLU's variance. What fails is the
inference that this makes the *integral* of the conditioned H2 truncation a
good approximation. In the realized deep field, higher posterior chaoses
must cancel the low-chaos integral at roughly three decimal orders in
amplitude.

This is why no clipping, shrinkage scalar, amplitude grid, or nonnegative
projection is licensed. Such a repair would be target-driven and would not
evaluate the theorem proposed by the expert.

## Remaining KLPQ gate

R2 does not reject the full posterior ReLU mean

```text
eta(u) = sqrt(v(u)) phi(m(u)/sqrt(v(u)))
       + m(u) Phi(m(u)/sqrt(v(u))).
```

It instead makes the next test sharper: evaluate `eta-c` densely and add the
analytic `I(c)`. If the full controlled integral fails, the pure K31 KLPQ
continuation is dead. If it succeeds, H2 is merely a variance-reduction
device and never an estimator on its own.

## Seal note

The first post-seal scorer invocation opened the fixed target members but
stopped at a target-shape assertion before computing any metric. Both target
archives store final targets directly as `(rows,256)`. The only mechanical
retry removed an erroneous trailing `-1` index; no formula, row, variant,
threshold, or target-dependent choice changed. The final receipt records
this explicitly.

## Artifacts

- preregistration:
  `PREREG_KLPQ_H2_ANALYTIC_R2_20260808.md`
- target-free source:
  `run_klpq_h2_analytic_r2_targetfree_20260808.py`
- convolution receipt:
  `runtime/artifacts/klpq_h2_convolution_r2_component_20260808.json`
- sealed target-free capture:
  `runtime/artifacts/klpq_h2_analytic_r2_targetfree_20260808.npz`
- sealed target-free report:
  `runtime/artifacts/klpq_h2_analytic_r2_targetfree_20260808.json`
- post-seal receipt:
  `runtime/artifacts/klpq_h2_analytic_r2_postseal_20260808.json`
