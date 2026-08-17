# Exact great-circle partition R2 verdict

Date: 2026-07-29

Verdict: **killed by both compute capacity and the preregistered two-family
accuracy gate**.

Evidence label: **component**. This was a weights-only exact partition
capture followed by a post-seal score on one Full and one Generated row. It
is not broad statistical, measured-whole, or remote evidence.

## Exactness and interval growth

For one Full row-3 Haar plane, the propagated piecewise-sinusoidal
representation matched direct dense propagation at 257 random angles with
maximum absolute error `1.4021e-15`. Its analytic final integral matched a
`q=262144` midpoint integral with maximum absolute error `1.9940e-10` after
the Gaussian radial factor.

The common partition did not explode exponentially, but it grew steadily:

```text
depth                 0      1      2      3      8      16      24      31
intervals            513   1015   1545   2057   4635    8571   12351   15853
```

Across four planes on each of Full row 3 and Generated row 35, final
partitions ranged from `14,557` to `17,811` cells.

## Compute capacity

An exact plane requires two dense coefficient propagations per incoming
cell. Across depth 32, the eight-plane mean was `497,573.5` dense
row-layers, equivalent to `15,549.2` ordinary propagated nodes per layer.

Even granting the entire `272B` budget at the current K146 linear
row-layer price—and charging nothing for root discovery, sorting, masking,
integration, or estimator closure—fits only `9.08` planes. This is an
optimistic upper bound, not a physical receipt.

## Accuracy

The target-free capture froze exact prefixes of 1, 2, and 4 Haar planes.
The post-seal four-plane result was:

| row | exact 4-plane MSE | vs Kerdock K8 | vs K146/m17 |
|---|---:|---:|---:|
| Full 3 | `1.48424e-3` | `803.76x` | `12,113.81x` |
| Generated 35 | `9.77909e-4` | `303.95x` | `9,716.63x` |

The one- and two-plane prefixes also lost by hundreds to tens of thousands
of times. Exact angular integration removes within-plane quadrature error,
but the challenge signal is dominated by between-plane coverage. The budget
cannot buy enough exact planes.

## Scope and correction note

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.

`exact_circle_ensemble_r2_postseal_20260729.json` is invalid: its first
scorer version accidentally indexed the two-dimensional Full final-target
bank as though it contained all layers. The corrected scorer hash is
`67fd21749c686fa452b4804f7eb7edea005b9f692d9eebc27e066dc5f59fbabc`;
the authoritative receipt is
`exact_circle_ensemble_r2_postseal_corrected_20260729.json`, SHA-256
`e2bc3a8ef15d7010a7bd9086e23a5c664540ade36733797680ac55c34e5decba`.

Other principal seals:

- exact partition source:
  `7fcfcfbfcca09139ed82fea95312acd847122b1997941af93a23f3b2b52d2378`
- first-plane exactness receipt:
  `10670cfa496d814195bb584197dca4169a1bf41dd1206d7ecb557e5633601900`
- preregistration:
  `e12be46f239de64bf4bb049ceaa6affd125f48b1da4ff609880ea6f3948cc9fe`
- target-free capture source:
  `55c373dfdc3c10cf5984e79b817b631903b803a6e83bd99b52a4979bff0f25ba`
- target-free capture:
  `a02f978820ce18a0284e270e6373c0df20c31e11eb8adedb46c04f27ee0e9b72`

Do not reopen fixed or exact great-circle ensembles without a mechanism that
reduces between-plane variance by orders of magnitude, such as a genuinely
weight-conditioned plane rule with cross-family evidence.
