# K129 cheap accuracy R1 — mixed verdict

Date: 2026-07-29.

Evidence label: **component development diagnostic**. Count and adjusted-score
economics are **projections**. No FlopScope session, physical row, benchmark
lane, estimator edit, package, upload, submission, or remote action occurred.

## Bottom line

There is one immediate, compute-neutral K129 improvement and one new
target-blind accuracy mechanism that passes the requested raw-error gate but
does not yet pay for itself safely.

### Accept: O0 signed-mean lambda `0.0075`

The lower-K endpoint grid had already frozen O0 at lambda `0`, `0.0075`, and
`0.015`. The old two-arm selection chose lambda zero after scoring the blended
K162 estimator. That conclusion does not transfer to the new O0-only
statistic.

Changing only the existing final signed-mean correction constant from zero to
`0.0075` gives:

| bank | lambda-0 raw | lambda-.0075 raw | ratio | gain |
|---|---:|---:|---:|---:|
| Full100 | `2.550453740e-7` | `2.532020737e-7` | `0.992772657` | `0.7227%` |
| Generated64 corrected | `2.605975144e-7` | `2.597656748e-7` | `0.996807952` | `0.3192%` |

Both trajectory sets were target-free freezes, and the lambda-zero O0 hash
matches the K129 broad q0 exactly. The current readout already computes
`proxy_ez`, `sample_ez`, their difference, the affine add, and the ReLU with
lambda zero. Replacing the scalar therefore projects to the same counted
graph and call count.

**Decimal correction:** the first version of this note mislabeled the
`lambda0075` endpoint as `0.075`. The authoritative stored grid is
`[0, 0.0075, 0.015]`. The frozen arrays, ratios, and gains above have not
changed; no `0.075` candidate is recommended or tested.

**Decision:** compose lambda `0.0075` into the next K129 source before any new
broad capture. It still needs the ordinary current-source association and
runtime gates; this note is not a measured whole.

### Accuracy pass, deployment hold: two-basis O1 proxy

A deterministic two-step OMP used only Synthetic1024 rows `0:512` and no
challenge targets. Its proxy objective was the equal mean of the complete
129-basis O0 and O1 endpoint populations. It selected:

```text
O1 bases       [70, 45]
coefficients   [0.008310604667785519, 0.007937092404688841]

candidate = q0
          + c70 * (O1_70 - q0)
          + c45 * (O1_45 - q0)
```

The candidate was replayed on the named Full100 and on disjoint Synthetic
rows `512:1024`:

| gate | candidate/control | raw gain | p95 row ratio |
|---|---:|---:|---:|
| Full100, grafted onto exact current q0 | `0.982863425` | `1.7137%` | `1.22336` |
| Full100, historical same graph | `0.982165392` | `1.7835%` | `1.23966` |
| Synthetic512 corrected, same graph | `0.984233842` | `1.5766%` | `1.18966` |

Thus the mechanism clears `>=1.5%` raw gain on both process-separated
families. The Full current-q0 result remains a cross-graph graft because no
current K129 O1-basis capture exists. It is not broad statistical evidence
for an executable successor.

The exact K146-minus-K129 count difference, divided by 17 O1 bases, gives an
affine projection of `0.975050379B` counted operations per basis, or
`1.950100758B` for this pair. That leaves effectively no residual margin:

```text
count-only adjusted factor, Full       0.996143
count-only adjusted factor, Synthetic  0.997531
extra residual before break-even
    Full                               0.00566 s
    Synthetic                          0.00362 s
```

A separate O1 arm will almost certainly consume more than `3.6ms` of new
request residual. Therefore the result does **not** establish a strict
`<=2B` effective increment and does not robustly move the remote-calibrated
K129 band below `1.2e-7`.

**Decision:** retain the pair as a real target-blind accuracy component, but
do not interrupt the K129 package/checkpoint path to implement it. Reopen only
if a static packed-row design appends the two O1 bases with essentially no new
request sites, or if it can replace equal counted work elsewhere.

## Evidence boundary

- The selection rule is deterministically target-free, but all development
  targets were already open elsewhere in the capsule. This is not virgin
  confirmation.
- The replay freezes its candidate arrays before opening targets within the
  replay itself.
- The two-basis Full result uses historical production-aligned basis
  responses grafted onto exact current q0.
- The lambda result uses exact already-frozen current q0 arrays on both named
  broad banks.
- No current-source estimator prediction or physical price was produced.

## Authoritative artifacts

- `replay_k129_cheap_accuracy_r1_20260729.py`
- `k129_cheap_accuracy_r1_targetfree_seal_20260729.npz`
  (`80bb440d7cfdf5f9b2ff5b7de534fa3c27c602133128389d6cf9c0f791e30b58`)
- `k129_cheap_accuracy_r1_component_score_20260729.json`
