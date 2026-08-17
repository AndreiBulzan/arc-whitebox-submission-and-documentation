# Sensitivity-directed mean-fold compression, K146 R1

Date: 2026-07-28  
Evidence sought: `component` accuracy and `projection` cost only.

## Question

The live K146 graph compresses the analytic width-232 support to width 216
for layers 12--20.  It ranks coordinates by post-ReLU second moment, then
replaces every omitted coordinate by its analytic mean in the next affine
map.

That ranking pays twice for a large deterministic mean: it retains the
coordinate even though the existing fold already transmits its mean.  The
quantity actually lost by omission is the fluctuation.  R1 therefore ranks
coordinate `i` at layer `l` by

```text
post_variance[l, i] * backward_observability[l, i].
```

The observability is target-free.  Starting at layer 30 with the rowwise
squared norm of `W31`, recurse backwards through the analytic screened graph:

```text
q_l = (W_{l+1} ** 2) @ (Phi(alpha_{l+1}) * q_{l+1}).
```

`q_l` is rescaled by a positive layerwise scalar only to avoid underflow.
The Gaussian gate probability is `Phi(alpha)`, not a fitted coefficient.

## Frozen rows and candidates

No target is opened until every prediction below is sealed.

```text
Full       211, 223, 227, 239
Generated   13,  20,  24,  27
```

The K146 support, H2 repair, layer-4 snap, lambda-zero arm blend, late
192-wide energy selection, final 176-wide selection, mean restoration, and
gamma readout are unchanged.

```text
energy_s12_w216     incumbent second-moment rule; start layer 12; width 216
variance_s12_w216   local post-variance only; start layer 12; width 216
obsvar_s12_w216     variance * observability; start layer 12; width 216
obsvar_s12_w200     variance * observability; start layer 12; width 200
obsvar_s12_w192     variance * observability; start layer 12; width 192
obsvar_s07_w192     variance * observability; start layer  7; width 192
obsvar_s07_w184     variance * observability; start layer  7; width 184
obsvar_s07_w176     variance * observability; start layer  7; width 176
```

The early candidates do not remove a layer.  They enter the same
mean-folded state at W7 rather than W12 and fold omitted analytic means
through W8--W21.

## Preregistered cost projection

Let `R=74,752`, `n` be compressed width, and `d=232-n`.  For a start layer
`s`, the directly changed sampled graph is priced with the released
FlopScope rule `M*N*(2*K-1)`:

```text
boundary:       R*n*(2*232-1) + R*n
square layers:  (20-s) * [R*n*(2*n-1) + 2*R*n]
transition:     R*200*(2*n-1)
fold vectors:   (20-s)*[d + n*(2*d-1)] + [d + 200*(2*d-1)]
```

Relative to the sealed steady count `144,013,215,420`, this gives:

| candidate | projected counted FLOPs | reduction |
|---|---:|---:|
| energy/variance/obsvar s12 w216 | 144,013,215,420 | 0.00% |
| obsvar s12 w200 | 135,009,540,300 | 6.25% |
| obsvar s12 w192 | 130,737,337,812 | 9.22% |
| obsvar s07 w192 | 118,131,236,572 | 17.97% |
| obsvar s07 w184 | 111,760,605,492 | 22.40% |
| obsvar s07 w176 | 105,638,745,740 | 26.65% |

These are exact algebraic projections for the replaced numerical operations,
not physical receipts.  Request/free/layout effects and wall are not priced.
Holding the remotely observed non-count residual contribution fixed at
approximately 26.3B effective units gives an effective range of roughly
131.9B--170.3B across the grid.

## Go/kill gates

The incumbent control must first associate with the sealed Full0 R17
baseline at relative RMSE at most `2e-6` and maximum absolute error at most
`3.2e-5`.

For the ranking mechanism:

- `obsvar_s12_w216` must have pooled MSE no worse than
  `0.98 * energy_s12_w216` in **each** family, and its worst family row ratio
  must be below `1.25`.
- Otherwise sensitivity ranking is killed without narrowing.

For a compressed candidate to advance:

- it must improve or tie the control on at least two of four rows in each
  family;
- no row ratio may exceed `2.0`;
- and its family-pooled raw ratio must fit the conservative remote checkpoint
  arithmetic in both families:

```text
s12 w192: ratio <= 0.995
s07 w192: ratio <= 1.075
s07 w184: ratio <= 1.130
s07 w176: ratio <= 1.180
```

`s12 w200` is diagnostic; it advances only if its pooled ratio is below
`1.02` in both families.

There is no coefficient fit, rowwise routing, post-target candidate choice,
or reuse of the eight rows for a successor.  Failure closes this exact
variance/observability ranking.

