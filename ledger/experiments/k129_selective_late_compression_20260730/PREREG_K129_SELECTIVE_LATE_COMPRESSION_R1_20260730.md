# K129 selective late-compression R1 preregistration

Date: 2026-07-30

This is one fixed, target-free K129/R21 accuracy falsifier. There is no
geometry grid, coefficient fit, target-dependent selection, physical
FlopScope run, package action, upload, submission, or remote action.

## Fixed estimator

The control is the production-aligned K129 O0 statistic with endpoint
`lambda=0.0075`.

The candidate differs only as follows:

```text
late layers 24..29 keep       160 instead of 192
late layer 30 keep            192 unchanged
final layer 31 keep           160 instead of 176
final output scale            1.000025
```

Every keep set is selected by the incumbent target-free activation-energy
rule on the candidate's own rolled state. Layer 30 is deliberately left
wide. The layer-31 omitted-sample-mean restoration remains exactly the
production spelling.

## Fixed rows

```text
Full       indices 0,1,2,3,4,5,6,7
Generated  indices 0,1,2,3,4,5,6,7
```

Both control and candidate predictions for all sixteen rows must be written
to one target-free seal before any target member is opened. A distinct
post-seal scorer may then open only those fixed targets and Generated label
noise.

## Exact count projection

The current R21 static slopes are:

```text
one late layer, one retained channel      18,435,963
final layer, one retained channel         20,829,898
```

Therefore:

```text
6 * (192-160) * 18,435,963       3,539,704,896
    (176-160) * 20,829,898         333,278,368
total projected count saving             3,872,983,264
```

This is an exact current-graph primary-kernel projection, not a measured
whole ledger. It excludes any small count/call changes outside those named
contractions and does not re-credit R21's existing `89,949,888` cleanup.

Using the conservative R21 upper anchor `1.2321377e-7` and projected public
effective work `146.990043463B`, unchanged raw error gives
`1.199672585e-7`. Multiplying the separately frozen output-scale ratio is
only a sensitivity (`1.197421405e-7`), not evidence for this changed
trajectory.

## Hard gate

For each family, compute the pooled candidate/control final-layer raw-MSE
ratio. Generated MSE uses the same mean label-noise subtraction in numerator
and denominator.

```text
maximum permitted ratio in each family    1.000273
```

If either family exceeds `1.000273`, kill this exact geometry immediately
and perform no broader capture, estimator integration, physical benchmark,
package, or remote action. If both pass, retain only as a component lead for
parent review; this tiny sixteen-row gate cannot establish broad or remote
performance.

