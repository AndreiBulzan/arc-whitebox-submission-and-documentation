# K129 mixed64/D6 paid-q0 anchor — R2 preregistration

Date: 2026-08-03

Evidence before execution: the alternate proxy and teacher are sealed
**target-free**; all score and compute consequences are **projections**.

## Single change

The broad R5 feature is `alternate_D6 - mean(q0_basis_D6)`.  Replace only the
reference with the already-paid exact q0 endpoint:

```text
alternate_D6 - q0_endpoint
```

Refit the same 64 scalar coefficients with ridge-relative `1.0` and the same
four-fold, family-balanced target-free teacher objective.  Frames, bases,
directions, literal independent repair, depth, q0 output, teacher, and output
scale remain fixed.

If successful, runtime no longer needs to observe q0 layer-5 moments or run
129 q0 diagonal continuations.  Only the 64 alternate continuations remain.

## Blocking prior-art result

Searches covered `paid anchor`, `q0_proxy_mean`, `q0 endpoint`, `D6 anchor`,
and the multidepth paid-anchor scout.  R13 tested tiny post-hoc frame means
(4--32 bases per frame) anchored to q0 and transferred poorly.  It did not
test the independently repaired, broad-confirmed literal mixed64 feature nor
its 64-dimensional ridge.  The mixed64 R5 and matched48 work deliberately
retained the complete q0 proxy mean.  Therefore this exact observable is new,
while R13 is the explicit warning that it receives one quick test only.

## Frozen procedure and gates

1. Fit on the already sealed Full100/Generated128 target-free four-frame
   teacher, with ridge `1.0`; seal predictions before challenge targets are
   read.
2. Score the fixed seal using the existing broad scorer.
3. Promote only if both families improve over q0 and the worse central
   adjusted projection is at most `1.10e-7` after charging the R31 delta, the
   exact unique-frame saving, and a conservative linear 64/193 share of the
   prior `1.4B` diagonal-continuation allowance.
4. Kill unchanged otherwise.  No extra ridge, support, or coefficient search
   follows a failure.

The price model in (3) is still a **projection** until a whole physical
receipt exists.
