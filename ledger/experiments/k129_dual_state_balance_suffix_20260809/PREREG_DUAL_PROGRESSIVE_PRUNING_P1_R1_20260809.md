# Dual-frame progressive-pruning structural oracle P1

Date: 2026-08-09

## Question

Can the broad checkpoint-16 D3 q0/polar selector commit a large fraction of
its line choices at checkpoints 8--14, while retaining the checkpoint-16
selection geometry?  A passing result would remove duplicate q/p prefix work
for committed lines without introducing targets, learned network labels, or
new trajectories.

## Evidence boundary

Map only `weights.npy` from the official Mini100 archive.  Never map, read, or
score targets in P1.  Use eight public rows for the structural choice and
eight holdout rows only for frozen structural transfer:

```text
public:  4, 11, 18, 25, 32, 39, 46, 49
holdout: 50, 57, 64, 71, 78, 85, 92, 99
```

The evidence label is `component`; all cost numbers are `projection`.

## Frozen selector

At checkpoints `6,8,10,12,14,16`, reproduce the broad D3 feature exactly:

- whole-pair q-minus-p checkpoint-state means;
- whole-pair q-minus-p checkpoint-state second moments;
- the same first and second moments after the 32 highest-energy exact
  next-gate probes;
- the count coordinate;
- the existing deterministic greedy discrepancy solver.

For every checkpoint retain the line signs and the normalized one-coordinate
flip margin after the two coordinate passes.

## Frozen early-commit rule bank

Evaluate these target-free rules:

1. checkpoint-8 top-margin fractions `25%, 50%, 75%`;
2. checkpoint-8 signs stable from checkpoint 6, intersected with those same
   top-margin fractions;
3. checkpoint-10 signs stable from checkpoint 8, with top-margin fractions
   `50%, 75%`;
4. sequential stability at checkpoints `8,10,12,14`, committing an
   uncommitted line when its sign matches the preceding checkpoint and its
   current margin is in the top `50%` or `25%` respectively;
5. sequential stability without a margin threshold.

For each rule, hold committed signs fixed at checkpoint 16 and optimize only
the remaining signs with the same deterministic greedy/two-pass solver.

## Target-free choice and gates

On the eight public rows, choose the lowest projected effective-compute rule
that satisfies all of:

- mean committed fraction at least `0.50`;
- mean constrained/unconstrained checkpoint-16 residual-squared ratio at
  most `1.05` and maximum at most `1.15`;
- mean constrained-sign agreement with the unconstrained checkpoint-16 rule
  at least `0.95`.

The one chosen rule then transfers unchanged to the eight holdout rows and
must satisfy the same mean gates, with maximum residual ratio at most `1.20`.
Its projected effective compute must be at most `195B`, using the sealed D3
model

```text
212.429423672B - sum_c committed_fraction_at_c * 4B * (16-c).
```

If no rule passes, close progressive early commitment for this D3 spelling.
If one passes, build a separate exact suffix-output component; do not infer
accuracy from selection agreement alone.

No physical row, package, upload, submission, or remote action is authorized.
