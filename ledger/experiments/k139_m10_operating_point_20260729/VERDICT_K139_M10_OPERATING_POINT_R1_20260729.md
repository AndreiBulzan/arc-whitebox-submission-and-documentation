# K139 / m10 operating point R1 — killed at the fast gate

Date: 2026-07-29.

Evidence label: **component** accuracy gate on four already-used Full rows and
four already-used Generated rows.  Count/effective/adjusted figures are
**projection**.  There was no FlopScope run, physical row, package, network,
upload, submission, or remote action.

## Frozen target-free identity

The exact K146 lower-K greedy-omission plus deterministic-local-exchange
algorithm was reproduced from the pinned target-free Gram.  Its m109, m33,
and m17 outputs matched their frozen identities.  With retained size fixed
in advance at ten, it produced:

```text
S10 = [3, 6, 11, 42, 50, 58, 64, 102, 112, 128]
SHA-256 (little-endian int16)
    b79258171ed97d6189a0ca6d1a003d7039c6cc840952a0a9db48295905d2a39b
```

No target, weight, seed, or prediction member was opened before this support
was serialized and hash-sealed.

## Actual final-output gate

The production-aligned K146 CUDA replay was parameterized to m10 while
retaining all repairs, widths, layer ordering, late energy selection,
lambda-zero gamma readout, and float32 propagation.  The sole final was the
literal `(129*q0 + 10*q1)/139` blend; both arms were persisted.

| family | rows | candidate/control pooled raw | max row ratio | improved |
|---|---:|---:|---:|---:|
| Full | 4 | `0.893715` | `0.978109` | 4/4 |
| Generated | 4 | `1.138452` | `1.281087` | 1/4 |

The Generated-family reversal is decisive.  It fails the fixed `<=0.98`
pooled gate despite no individual catastrophic row.  Per preregistration,
K139/m10 is killed without Full100/Generated64 broadening.

## Compute economics

Exact geometry is `129 + 10 = 139` bases and `71,168` propagated signed
rows.  Matched K146/K162 count anchors give the whole-count projection:

```text
K146 banked count                         144.013469372 B
projected variable count / O1 basis         0.971176037 B
K139 projected count                      137.215237114 B
```

Remote effective-compute anchors, followed by the already banked
prediction-preserving decoder/layout saving, give:

```text
K139 before banked rewrite                164.818029887 B
K139 after banked rewrite                 162.934629339 B
projected multiplier                            0.599024
remote-anchor port-only adjusted            1.253344e-7
raw ratio required to reach 1.2e-7               0.957439
```

The worse observed family ratio `1.138452` would instead project to
`1.426872e-7`.  These are linear projections from matched count and remote
aggregate anchors, not a K139 measured whole.

## Decision

Do not implement, physically meter, package, or broaden this unchanged
K139/m10 identity.  Its compute saving is real enough to be interesting, but
the target-free S10 support loses transfer accuracy faster than the score
multiplier improves.

## Artifact hashes

- preregistration:
  `6e0a30ddaf2da876783276fdbb72fba156abfceb9ba9e3572a742a809bb78b5a`
- support freeze source:
  `802a1c37d7e0b7c0ac80441c13df02a2d52ccf354d6121250e30a73f1021a3aa`
- support archive:
  `3f9df408c65494e312524bc94c3fbcb31eb32ce8c8afaacbc1aa19ed4dc4b833`
- support receipt:
  `4d2b3f80c4ad905e7766c766e1fb16cd73e366e8f101656df5c262c3f40bad0e`
- CUDA adapter:
  `932c7874c271af7e524e2076261b58b93be4487e491b99df6c4874e3e666787d`
- target-free capture source:
  `1d550408f5dea45f0ced123e33f743f342515325c4018fc3ac56e88134adfe14`
- target-free prediction archive:
  `30058771f63a70e2d7edb89da321d718fe11d99d902733d8873c6e033a79bb85`
- target-free manifest:
  `4c4c7d55d15ddafa722fbb2021712b4ab48e0a21050e145d000ec8d78c97525c`
- post-seal scorer:
  `9de4f178bf7412546d258f08ceb791819d13598854617e594e3a2ec4ae152b04`
- score arrays:
  `6b43e8392ee7fbc02ae9816fce302432b92262b02ab6a451ee7cad119f07c19e`
- score report:
  `f25afc822519fc4a8b45533023b919cb3d76519bfd9955a328013f9e04f40cc8`

