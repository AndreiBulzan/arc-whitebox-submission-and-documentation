# K129/R21 + frozen Stage-A gauge-cell bridge R1

Date: 2026-07-29

Evidence label: **projection / source-static audit**. No FlopScope, physical, package, or remote work was run for this audit.

## Verdict

**Killed before implementation.** The frozen step-20 gauge cell cannot be attached as a `<=0.5B` K129/R21 sidecar using already-paid production state, and the existing artifacts cannot supply an honest held-Full plus Generated score for such a bridge.

This is not a retune of the closed Stage-A family. It is a composition audit of the exact R3-selected step-20 state:

```text
checkpoint SHA-256  ec81cf3d3b18a8d46379b3167fc1d12fcac3c2ed874c417ebf0164e67e5d5c2d
model-state SHA-256 fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
K129/R21 source      ff86e02a0724dcd28d97ca883ff649c1d1916390d8bceec23db4f000e8bde9d4
```

## 1. The trained inference surface is not present in K129

The step-20 cell was only measured in **teacher-forced** mode. At every layer after zero it consumes the exact population `official_alm[l-1]` and `m2[l-1]` (Stage-A trainer lines 262--280), then uses an explicit dense `256x256` covariance recurrence (lines 282--291 and 416--435). Those are not values the K129 production graph produces.

K129 instead obtains `proxy_raw`, `seconds`, `screens`, and `screened_means` from its compact closure, then immediately reduces the state into its arm-specific front/middle path. It has no retained all-layer `(mean, second, covariance)` population state, and it releases the middle transition before returning the final `(32,256)` prediction. See K129 parent lines 565--747. A sidecar therefore has only two lawful choices:

1. reconstruct a new full 32-layer state from weights; or
2. substitute the compact cloud summaries for the missing teacher state.

The first is new work, not reuse. The second is an untrained input distribution: it is not the step-20 model that was evaluated. Its nearest available spelling is the fully self-predicted free rollout, which was killed at Full development with final post-mean MSE `1.291857981e-2`, 12,919 times its `1e-6` continuation ceiling. It cannot plausibly satisfy the requested bridge ratio `<=0.9706` on both families.

## 2. Even the hard lower bound is over the sidecar cap

The preregistered cell requires four state-message contractions for each of the 31 noninitial transitions: signed, squared/even, magnitude, and 16-channel probe. Its own accounting gives one `256x256` by `256x64` message as `8,372,224` operations. The three 64-channel messages plus the probe message therefore have the exact arithmetic lower bound:

```text
31 * (3 * 8,372,224 + 2,093,056) = 843,501,568 counted operations.
```

That is `0.843502B`, already 1.69x the entire `0.5B` allowance, before the initial Gram/initializer, dense covariance transport, pointwise cell, importance recursion, normalization, or readout application. The original Stage-A economics lists the four messages as about `1.038B` because it prices the 64-channel probe conservatively; in either spelling the cap fails on message propagation alone. K129 has not previously paid these channel-bearing contractions: its K-wide cloud contractions operate on different operands and do not construct `state[256,64]` or `probe[256,16]`.

## 3. No existing sealed cache can score the proposed composition

The Stage-A cache manifest is explicitly `reference_only`. It contains weights plus six population-moment arrays for train/development; it contains no K129/R21 predictions, no captured K129 intermediate state, and no sidecar predictions. Full held `800..999` values were deliberately never opened. Generated labels were unavailable to Stage A and never read.

The only frozen step-20 prediction archive is the failed **free rollout** on Full development `700..799`; it is neither a K129 composition nor a held/Generated prediction set. Consequently, a purported score assembled from it would be neither held nor process-separated and would not meet the capsule validation discipline.

## Decision

Do not implement, score, or physically meter this bridge. Reopen only with a genuinely new sidecar that (a) consumes an already materialized K129 observable, (b) has a static counted-work bound at or below `0.5B`, and (c) freezes new composition predictions before opening the held-Full and Generated targets. The Stage-A step-20 state itself does not meet those conditions.
