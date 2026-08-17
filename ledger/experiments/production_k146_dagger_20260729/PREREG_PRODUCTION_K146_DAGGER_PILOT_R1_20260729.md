# Production K146 rolled-state stabilizer — pilot R1 preregistration

Date: 2026-07-29

Evidence sought: **component**. This is an ordinary-CUDA accuracy falsifier.
It authorizes no FlopScope run, physical benchmark, package, upload,
submission, or remote action.

## Question

Does a correction learned on the actual rolled K146/m17 arm-and-basis state
improve the final estimator, rather than merely predicting a detached
endpoint residual?

At production layers `4,8,12,16,20,24,28,30`, R1 forms six
permutation-equivariant, positive-gauge-covariant vectors from the current
K146 basis means:

1. matched O0 support17 minus complete O0;
2. O1 support17 minus matched O0 support17;
3. O0 zero basis minus O0 nonzero-basis mean;
4. the first balanced O0 Walsh contrast;
5. the second balanced O0 Walsh contrast; and
6. O1 basis standard deviation minus O0 basis standard deviation.

A six-coefficient shared linear correction is fitted at each checkpoint to
the exact Full training post-activation mean. The correction is applied as a
positive coordinate-wise scale to every current particle in both arms.
Later checkpoint fits therefore see the candidate's own rolled trajectory.
No target, MLP id, seed, or neuron id is an input.

The fixed global shrink grid is:

```text
eta = 0.25, 0.50, 0.75, 1.00
```

Each eta is trained sequentially on its own rollout. Full development final
MSE selects one eta, with the unmodified baseline included as eta zero.

## Pilot split

```text
Full train       0..11
Full development 12..15
Full held        16..19
Generated held    0..3
```

Only Full train/development all-layer means may be opened before selection.
Full-held and Generated predictions are sealed before a separate scorer
opens either held target family.

The pilot is deliberately small. It is a kill screen, not broad evidence.
A positive pilot would license the prespecified `48/16/16` Full acquisition
and disjoint Generated guard; it would not license estimator integration.

## Hard pilot gate

Continue only if all hold:

```text
Full-held pooled raw-MSE ratio       <= 0.88
Generated pooled corrected-MSE ratio <= 0.88
row-ratio p95 in each family         <= 1.25
all predictions finite
projected incremental count          <= 2B
```

Generated pooled MSE is corrected by subtracting the published
per-network label-noise MSE from both baseline and candidate pooled losses.
Generated row-ratio tails use observed losses because a per-row
noise-subtracted denominator can be non-positive.

Failure kills this exact six-feature/eight-checkpoint multiplicative
rolled-state spelling immediately. No coefficient, checkpoint, row, or
feature tuning follows a failed held gate.
