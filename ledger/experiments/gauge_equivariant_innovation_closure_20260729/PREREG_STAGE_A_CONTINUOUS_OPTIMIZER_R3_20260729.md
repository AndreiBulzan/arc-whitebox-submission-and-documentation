# Gauge-closure Stage-A continuous-optimizer R3

Date: 2026-07-29

Status: **final protocol, frozen before code or execution**.

Evidence sought: **component**.  R3 authorizes no Full-held or Generated
value access, Stage B, FlopScope session, physical row, estimator/package
work, upload, submission, or remote action.

If R3 fails, this experiment family closes with no further retry.

## Decision isolated by R2

R1 reached a stable step-20 Full-development checkpoint:

```text
late-16 innovation       0.6039600626
late-16 signed premean   0.5962199995
model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

R2 loaded that model but necessarily reset the missing AdamW moment buffers.
It immediately deteriorated.  R3 asks only whether continuing the original
optimizer trajectory—without a state reset or a new loss—reaches the hard
Stage-A development gate.

## Exact replay gate

Start from scratch with the R1 model initialization and:

```text
seed                  2026072921
batch                          8
optimizer                  AdamW
learning rate              1e-4
weight decay               1e-5
gradient clip               1.0
loss                original R1
```

Use the original R1 loss exactly.  There is no R2 late-depth weight and no
other loss, architecture, feature, covariance, message, state, bound,
normalization, split, or optimizer change.

Recreate the deterministic train batch stream from
`numpy.random.default_rng(seed)`.  Train through step 20 with one continuous
AdamW instance.  Before step 21, require the complete model-state SHA-256 to
equal exactly:

```text
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

If it differs by one bit, stop immediately, record replay failure, and do
not continue.

## One continuous run

If and only if replay passes, retain the same live model, optimizer moment
buffers, RNG, and batch stream.  Continue unchanged to total step 200:

```text
total optimizer steps       200
total train-MLP exposures 1,600
```

This means steps `21..200`, not another 200 steps.

Evaluate the complete Full development split `700..799` by ratio of pooled
error sums at:

```text
20, 40, 60, 80, 100, 120, 140, 160, 180, 200
```

Select the checkpoint minimizing:

```text
max(late16 innovation ratio, late16 signed-premean ratio)
```

with the later checkpoint winning an exact tie.

## Hard development verdict

R3 passes only if the selected checkpoint satisfies all:

```text
late-16 innovation ratio                  <= 0.40
late-16 signed-premean ratio              <= 0.40
innovation depth 16--23 ratio             <= 0.60
innovation depth 24--31 ratio             <= 0.60
finite predictions
zero bound violations
```

If it fails, close R3 and the gauge-closure training family.

If it passes, freeze the exact selected checkpoint and complete
Full-development prediction tensors with hashes.  Stop and report before
opening any Full-held or Generated value.  A development pass does not
itself authorize held evaluation.

