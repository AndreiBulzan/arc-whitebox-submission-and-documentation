# Gauge-closure Stage-A depth-localized continuation R2

Date: 2026-07-29

Status: **frozen before code or execution**.

Evidence sought: **component**.  R2 authorizes no Full-held or Generated
value access, Stage B, FlopScope session, physical row, estimator/package
work, upload, submission, or remote action.

## Motivation fixed from R1

R1's sole stabilization was stable and improved monotonically, but its
selected Full-development checkpoint missed the pilot headline:

```text
late-16 innovation ratio         0.6039600626
late-16 signed-premean ratio     0.5962199995

innovation depth 16--23          0.5578863374
innovation depth 24--31          0.6620288853
```

The remaining miss is localized to the last octet.  R2 tests one mechanism
only: deterministic late-depth loss emphasis.

## Immutable initialization

R2 initializes from:

```text
stage_a_stabilization_pilot_r1_20260729/checkpoint.pt
file SHA-256
c8dc4f2627ba9100e660638278449e8f8c1f21970a8d620088972e68d1a65a7c

model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

The R1 checkpoint contains model parameters but no serialized AdamW moment
buffers.  R2 therefore initializes a fresh AdamW state at zero moments while
keeping the exact frozen optimizer family and hyperparameters.  This is
declared before execution and is not a tunable choice.

## The one new mechanism

For target depth `d in 0..31`, define:

```text
late_weight(d) = 1 + 3 * (d / 31)^2
```

Multiply the existing innovation, post-mean, and log-second squared losses
at depth `d` by `late_weight(d)`, in addition to the unchanged target-free
importance weighting already present.

The signed-premean prediction at array position `k in 0..30` targets
preactivation depth `d=k+1`; multiply its existing squared loss by
`late_weight(k+1)`.

The K3 and K4 auxiliary losses are unchanged and receive no late-depth
weight.  Every scalar coefficient, normalization, bound, feature, message,
state channel, covariance update, and association is otherwise identical
to R1.

## Frozen continuation

```text
architecture                     c=64, r=16, hidden=128
train split                      Full 0..699
development split                Full 700..799
continuation optimizer steps     200
batch                            8
train-MLP exposures              1,600
optimizer                        AdamW
learning rate                    1.0e-4
weight decay                     1.0e-5
gradient clip                    1.0
seed                             2026072921
```

Recreate `numpy.random.default_rng(seed)` and deterministically consume the
first 20 R1 batch draws before the first R2 draw.  R2 therefore uses the
continuing batch stream (draws 21..220), not a replay of R1's first 20
batches.

There is no optimizer, learning-rate, batch, loss-coefficient, depth-weight,
architecture, feature, or seed grid.

Evaluate the complete development split by ratio of pooled error sums at
steps:

```text
20, 40, 60, 80, 100, 120, 140, 160, 180, 200
```

Select the checkpoint minimizing:

```text
max(late16 innovation ratio, late16 signed-premean ratio)
```

with the later checkpoint winning an exact tie.

## Hard development verdict

R2 passes only if the selected full-development checkpoint satisfies all:

```text
late-16 innovation ratio                  <= 0.40
late-16 signed-premean ratio              <= 0.40
innovation depth 16--23 ratio             <= 0.60
innovation depth 24--31 ratio             <= 0.60
finite predictions
zero bound violations
```

If the gate fails, close R2.  No additional continuation, optimizer repair,
or coefficient adjustment is licensed.

If it passes, freeze the exact selected checkpoint and complete
Full-development prediction tensors and report their hashes.  Stop and
report before any Full-held or Generated value is opened.  Passing R2 does
not itself authorize held evaluation.
