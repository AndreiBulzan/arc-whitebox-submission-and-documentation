# Orientation-pilot recycling R1 — verdict

Date: 2026-07-29

Evidence label: **component**.  Cost figures are **projections**.  No
FlopScope session, physical row, package, upload, submission, or remote
action occurred.

## Verdict

**Kill this spelling.** Reusing every eight-orientation pilot basis in the
final equal-weight estimate did not close the selector/main-support gap.
None of the 19 pre-frozen `(total K, pilot, main)` points passed the
cross-family signed-state gate.

The best balanced signed point was:

```text
total K / pilot / selected-main             148 / 5 / 59
Full signed-final-premean ratio                   1.08995
Generated signed-final-premean ratio              1.19476
```

Both families were worse than current K146, versus the required ratio
`<=0.80`.

The same point's first-order half-gate final readout improved both families:

```text
Full final-readout ratio                          0.86948
Generated noise-corrected ratio                   0.95764
effective-work projection                        174.989B
```

That is not a viable lead.  Its underlying signed state is worse on both
families, its row-ratio p95 is `2.58 / 3.41`, and the readout is only a
first-order correction applied to an existing K146 prediction—not an exact
candidate readout.  It therefore cannot justify a disjoint confirmation or
runtime integration.

## Interpretation

The six unselected orientations' pilots are not harmless samples from the
selected pair distribution. Equal-weight recycling pulls the estimate toward
a noisy eight-orientation finite-support mean faster than it reduces the
selected-pair error. Increasing pilot support simply trades selected-pair
resolution for broad-orientation sampling noise at fixed `K`.

The large label-using orientation oracle remains real, but this result
supports the prior diagnosis: the missing mechanism is reliable pair
identification, not bookkeeping around already-computed pilots.

## Artifacts

- preregistration:
  `PREREG_ORIENTATION_PILOT_RECYCLE_R1_20260729.md`
- target-free seal:
  `orientation_pilot_recycle_r1_targetfree_20260729.npz`
- target-free manifest:
  `orientation_pilot_recycle_r1_targetfree_20260729.json`
- post-seal score:
  `orientation_pilot_recycle_r1_postseal_score_20260729.json`
- capture/scorer:
  `capture_orientation_pilot_recycle_r1_targetfree_20260729.py`,
  `score_orientation_pilot_recycle_r1_postseal_20260729.py`

