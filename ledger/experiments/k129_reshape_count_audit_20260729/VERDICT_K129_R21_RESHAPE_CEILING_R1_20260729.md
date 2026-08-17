# K129 R21 reshape ceiling

Verdict: **kill before implementation**.

The headline premise was a row-type mix-up. In the exact R21 official-runner
receipt, `1,239` reshape calls costing `2,436,002,973` FLOPs belong to the
first, deferred-initialization row. The steady prediction has only `257`
reshape calls costing `192,063,040` FLOPs.

The completed remote K129 run used five initialized and ninety-five steady
rows. Applying that worker mix to R21 gives an impossible best-case ceiling:

```text
(5 * 2.436002973B + 95 * 0.192063040B) / 100
    = 0.304260037B mean count
```

That assumes every reshape vanishes, including all persistent-owner
construction and every prediction reshape, with no replacement operation or
request. A lawful producer-native-shape rewrite can only save less.

Consequences:

- The ceiling is below the requested first-cut target of `0.6B`.
- It supplies only `7.91%` of R21's approximately `3.848B` same-raw saving
  needed to reach `1.2e-7`.
- At the mapped R21 effective price, deleting all reshapes would improve the
  adjusted score by at most about `0.207%`, moving the existing projection
  band only from `1.22715--1.23213e-7` to approximately
  `1.22461--1.22958e-7`.

No physical prediction was run for this audit. The two row ledgers are
**measured whole** evidence from the already-existing exact-archive gate; the
five-worker amortization and score effect are explicitly **projection**.
Deep callsite attribution and implementation are not warranted for the
checkpoint.

Authoritative machine-readable receipt:
`k129_r21_reshape_ceiling_r1_20260729.json`.
