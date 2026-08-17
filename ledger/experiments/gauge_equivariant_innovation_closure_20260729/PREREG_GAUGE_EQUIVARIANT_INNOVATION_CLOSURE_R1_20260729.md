# Gauge-equivariant recurrent innovation closure R1

Date: 2026-07-29

Status: **static experiment specification; not yet trained**.

Evidence sought: **component**. R1 is an offline supervised
teacher-forced/free-rollout experiment. It authorizes no FlopScope session,
physical or timed row, estimator build, package, upload, submission, or
remote action.

## Decision question

Can one shared, inference-lawful recurrent state learn the layer-local
absolute-value innovations of a realized width-256 ReLU MLP well enough that
a completely free 32-layer rollout reaches:

```text
Full held final post-mean MSE             <= 2.5e-7
Generated noise-corrected final MSE       <= 2.5e-7
Full held signed-final-premean MSE        <= 5.0e-7
Generated signed-final-premean MSE        <= 5.0e-7
both-family row-MSE p95                    <= 5.0e-7
current-meter analytical work projection  <= 10B
```

Only the free rollout is candidate evidence. Teacher-forced results are an
early state-sufficiency gate.

## Exact supervised source

For post-ReLU means `m_l` and the next weight matrix `W_l`,

```text
pre_l       = m_l @ W_l
a_(l+1)     = E[|z_(l+1)|]
m_(l+1)     = 0.5 * pre_l + 0.5 * a_(l+1)
a_(l+1)     = 2 * official_alm[l+1] - official_alm[l] @ W_l.
```

Thus the 1,000-MLP higher-moments bank supplies 32,000 layer transitions
and approximately 8.2 million neuron-transition labels for the exact local
nonlinear source. R1 predicts the residual of a Gaussian/Hermite
absolute-moment anchor, not an endpoint residual and not the whole mean from
scratch.

The Full bank is used as a whole-MLP split:

```text
train       0..699
development 700..799
sealed held 800..999
```

Generated128 is a process-separated sealed free-rollout guard. Its targets
must not be opened in training or model selection. The phrase “use the
1,000-MLP bank” never means training on the held 200.

## One model, no architecture sweep

Use exactly one shared cell:

```text
per-neuron recurrent channels       c = 64
sign-bearing probe/factor channels  r = 16
shared pointwise hidden width           128
```

The inference state contains:

- post mean and post second moment;
- a full Gaussian/Price covariance anchor;
- 64 dimensionless recurrent channels;
- 16 sign-bearing connected-innovation/probe channels; and
- a target-free backward-importance channel.

The cell emits:

- a bounded residual for `E|z|`;
- a bounded post-second-moment update;
- auxiliary standardized preactivation K3/K4 queries; and
- the next recurrent/probe state.

The auxiliary K3/K4 heads are training constraints. The mean is always
formed through the exact ReLU identity above and clipped to:

```text
max(0, E[z]) <= E[ReLU(z)] <= sqrt(E[z^2]).
```

## Exact positive-gauge canonicalization

Let

```text
s_i = sqrt(E[h_i^2])
t_j = sqrt(sum_i s_i^2 W_ij^2)
A_ij = s_i W_ij / t_j
u_i = E[h_i] / s_i.
```

Under the exact hidden ReLU gauge

```text
h_i -> d_i h_i,       W_ij -> W_ij / d_i,       d_i > 0,
```

`A`, `u`, and every dimensionless state channel remain invariant. Predicted
means and second moments are restored with `t` and therefore transform
equivariantly. This is enforced algebraically, not learned from occasional
augmentations.

The first layer is initialized from `W0.T @ W0` and its shared row/column
contractions, never raw input-coordinate identities, preserving input
orthogonal invariance. Hidden-neuron permutations simply permute rows of
the shared state and adjacent axes of `A`.

## Message surface

At each layer use four explicit equivariant messages:

```text
A.T        @ H_signed
(A*A).T    @ H_even
abs(A).T   @ H_magnitude
A.T        @ L_probe
```

plus dimensionless analytic mean/variance/Hermite features, layer position,
and backward importance. The importance is computed target-free from the
visible suffix:

```text
b_31 = ones(256)
b_l  = sqrt((W_l*W_l) @ (b_(l+1)*b_(l+1)))
```

with an RMS normalization at each depth.

The 16 probe channels are born and recompressed by the shared cell. They
are the material addition over a marginal recurrence: they preserve signed,
cross-neuron path state that can carry all-distinct connected innovation.
They are supervised indirectly by the next preactivation K3/K4 queries and
directly through the absolute-innovation and all-layer mean losses.

## Two-stage training and the minimum gate

### Stage A — teacher-forced state-sufficiency

Insert exact previous `official_alm` and exact post second moments at every
transition, but roll the recurrent/probe state from layer zero without
teacher replacement. The analytic covariance anchor remains the same
inference-lawful predicted state; do not feed exact `M11/M21/M22/M31` into
the deployed input.

Loss terms:

```text
normalized absolute-innovation residual
next signed premean
next post mean and log second moment
standardized preactivation K3/K4 auxiliary queries
downstream-adjoint-weighted versions of the first three
```

Stop before free rollout unless the fixed Full development and sealed Full
held results both satisfy:

```text
late-16 innovation MSE / Gaussian-anchor MSE  <= 0.40
late-16 signed-premean MSE ratio               <= 0.40
every late depth-octet ratio                   <= 0.60
finite predictions and exact bound compliance
```

This gate is deliberately stronger than a small mean improvement. Rollout
can only lose information relative to teacher forcing.

### Stage B — scheduled free rollout

If Stage A passes, continue the same checkpoint with a single fixed schedule
for replacing true mean/second state by the predicted state:

```text
replacement probability: 0.75 -> 0.25 -> 0.0
```

The recurrent/probe state is never teacher-replaced. Supervise all 32 means,
all derived signed premeans, and the final row with a `1:1:4` relative loss
weight. Select one checkpoint only on Full development free-rollout final
MSE plus its row-p95 penalty.

Freeze all 200 Full-held and 128 Generated prediction arrays before either
evaluation target is opened. Score Full and Generated separately. The hard
gate at the top of this document is the only promotion rule.

There is no learning-rate, rank, channel, loss-weight, or architecture grid
after evaluation access. If Stage A passes but Stage B fails, one training
run may be extended under the already-fixed replacement schedule; no new
feature family is licensed.

## What is genuinely new

This experiment is not a continuation of the killed models:

1. `equivariant_moment_cell` R1 (`6.91e-4` Full / `7.09e-4`
   Generated) trained only a tiny diagonal marginal state by direct
   32-layer free rollout. It had no local teacher stage, exact positive-gauge
   canonicalization, downstream importance, or sign-bearing probe state.
2. CP-factor closure R2 (`~2.4e-5`) used hand-written Price births and a
   fixed compression rule. R1 learns births and recompression from the exact
   all-layer innovation and K3/K4 supervision.
3. Marginal-cumulant R3 (`~2.4e-5`) was a per-layer denoiser over an analytic
   state; it carried no recurrent signed connected state, and its
   within-layer K4 signal collapsed to correlation `~0.02`.
4. The K32 local learner saw 22 scalar cloud summaries and current-column
   invariants. It had neither the complete 1,000-MLP source labels nor the
   visible full path graph in a recurrent state.
5. Endpoint/hidden-cloud residual learners corrected a final estimate after
   the missing state had already been discarded. R1 is trained at the layer
   where each nonlinear source is born.

The new hypothesis is specifically that a learned, gauge-canonical,
sign-bearing recurrent state can transport the all-distinct interaction.
A larger marginal MLP, another endpoint head, or a target-fitted blend would
not test this hypothesis.

## Current FlopScope 0.9.1 economics

All economics are **projections** under the current FlopScope 0.9.1 cost
model; no historical 0.8 score or multiplier is used.

One dense `256x256` by `256xc` message at `c=64` has ordinary arithmetic:

```text
256 * 64 * (2*256 - 1) = 8,372,224
```

Four messages over 31 learned transitions:

```text
4 * 31 * 8,372,224 = 1.038B.
```

The full Price covariance anchor is approximately `2.1B`; pointwise cell,
rank-16 probe updates, reductions, and bounded readout are projected below
another `1--2B`. The pre-implementation envelope is therefore roughly
`4--6B` counted arithmetic, with all ordinary arrays far below 99 MiB.
Released request/residual tariffs are unmeasured, so `<=10B` is an
analytical gate, not a whole-estimator claim.

Even substantial participant overhead leaves room below the current
`27.2B` multiplier floor. At that floor, raw `1.0e-6` corresponds to
adjusted `1.0e-7`; the stated raw gate `2.5e-7` would correspond
arithmetically to `2.5e-8`. These are score projections, not receipts.

## Minimum bankable experiment

The next execution is exactly:

1. build one immutable cache containing only Full train/dev labels needed by
   the losses and weight-member hashes;
2. train the single `c=64,r=16` model through Stage A;
3. stop immediately if the teacher gate fails;
4. otherwise run the one scheduled Stage B;
5. seal Full-held and Generated free-rollout predictions;
6. score once and write the component verdict.

No physical pricing, estimator implementation, or second model is justified
before this result. A pass licenses a participant-safe static
FlopScope-0.9.1 graph/cap audit; it does not itself license a remote action.
