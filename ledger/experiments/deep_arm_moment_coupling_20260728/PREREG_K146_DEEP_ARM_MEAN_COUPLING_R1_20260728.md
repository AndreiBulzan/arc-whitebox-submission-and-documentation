# K146 deep arm-mean coupling R1

Date: 2026-07-28

Evidence sought: **component** numerical evidence. Any cost arithmetic is a
**projection**. No physical row, FlopScope session, package, network,
upload, or submission is authorized.

## Mechanism

The two K146 arms are independent structured quadrature views of the same
hidden distribution. The production graph moment-matches them at H2 and
again at layer 4, but lets their empirical means drift independently
thereafter.

At each selected post-ReLU checkpoint, compute arm means `m0`, `m1` and
their support-weighted pooled mean

```text
m = (129*m0 + 17*m1) / 146.
```

Then apply, separately to every particle in each arm,

```text
h_a <- max(0, h_a + beta * (m - m_a)).
```

Before the nonnegativity guard this preserves the pooled mean exactly. It
uses no target, fitted coefficient, analytic closure, random choice, or
per-row selection.

Frozen candidates:

- start layer 5, beta 0.25 / 0.50 / 1.00, through layer 21;
- start layer 12, beta 0.25 / 0.50 / 1.00, through layer 21;
- unchanged dense K146 control.

Smoke rows are Full 0,1 and Generated 2,4. Predictions must be sealed before
their targets are opened.

## Gate

Advance only if both families have pooled raw-MSE ratio `<=0.95` to the
control and no row ratio exceeds `1.25`. A pass is still only a component
lead; it next needs a disjoint two-family expansion and an exact lawful
ledger. A failure kills this literal coupling grid.
