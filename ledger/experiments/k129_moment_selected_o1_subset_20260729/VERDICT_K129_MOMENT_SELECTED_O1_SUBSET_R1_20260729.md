# K129 moment-selected O1 subset R1 — killed at held 4+4

Date: 2026-07-29

Evidence label: **component development diagnostic** for accuracy. This was
an ordinary target-free CUDA acquisition followed by post-seal scoring. No
FlopScope row, estimator edit, package, upload, or remote action occurred.

## Bottom line

Directly selecting O1 bases for final Edgeworth3433 reconstruction does not
repair the sparse-O1 family reversal. No selected one-, two-, or four-basis
support passed the preregistered `raw ratio <= .970` gate in both held
families, so this exact lane is closed without a broader capture.

The target-free exhaustive search on Full4+Generated4 selected:

```text
k=1  [111]
k=2  [9,108]
k=4  [6,42,111,128]
```

Only after those supports and both held-family isolated predictions were
written did scoring open the held targets. The fixed estimator was:

```text
candidate = 1.000025 * (
    q0(lambda=.0075)
    + .0734 * (isolated_O1_Edgeworth3433 - q0(lambda=.0075))
)
baseline = 1.000025 * q0(lambda=.0075)
```

## Held result

| selected support | Full held4 raw ratio | Generated held4 corrected raw ratio |
|---|---:|---:|
| `[111]` | `2.266428` | `1.811253` |
| `[9,108]` | `1.415074` | `0.785208` |
| `[6,42,111,128]` | `1.047979` | `1.307669` |

The two-basis support has a real-looking Generated-family gain but reverses
sharply on Full. The four-basis support is closest on Full but loses in both
families. None is near a two-family pass.

## What this establishes

The prior `[70,45]` negative was not merely a bad endpoint-based support
choice. Even exhaustive support selection against the exact full-17 final
Edgeworth readout on a fixed target-free split does not produce a
transferable sparse correction.

The per-basis views inside the joint m17 trajectory reproduced the prior
full-17 aggregate raw moments exactly (`max abs delta 0`). Isolated
repair/energy selection changed the selected readouts somewhat, but it was
not the controlling failure: for `k=4`, held target-free reconstruction MSE
to full17 was `7.1785e-6` in Full and `9.0634e-6` in Generated inside the
joint trajectory, versus `7.1782e-6` and `8.4083e-6` after isolated
execution. The family reversal survives the isolated spelling.

## Decision

Kill this exact moment-selected sparse-O1 lane. Do not run Full100,
Generated64, a FlopScope price row, or integration work. Reopening requires
a different statistic or estimator, not another support search over the
same equal-weight final raw-moment reconstruction.

## Sealed artifacts

- preregistration:
  `PREREG_K129_MOMENT_SELECTED_O1_SUBSET_R1_20260729.md`
  (`3c2345e8bac628e1365c9ccd993e86ddfac96747fe1987d3c8bb4f1ba3fd4f90`)
- Full all-17 capture:
  `full8_full17_per_basis_r1_targetfree_20260729.npz`
  (`9af7af0d214a6c18280a7e1d3114c5dbd8c8294fff6543a241e122538007ec4b`)
- Generated all-17 capture:
  `generated8_full17_per_basis_r1_targetfree_20260729.npz`
  (`db574f2a014d8d3579475ca7289936d8f5dfee6fae6ebeaf5847494a854f7018`)
- target-free selection:
  `selected_subsets_r1_targetfree_20260729.json`
  (`b621099041aeaba062322d3f19c60dedfb01f235b1acd3b5e5591d274976745c`)
- Full held isolated capture:
  `full_held4_selected_isolated_r1_targetfree_20260729.npz`
  (`fd8afd82f59f415699cb85fe4886d543db6393a3a75d93d382d753ed78a2bdb2`)
- Generated held isolated capture:
  `generated_held4_selected_isolated_r1_targetfree_20260729.npz`
  (`6faee08e031ae2b98b941fa4cfcf38274b7232f5f380c6b6b84a46a393f65d21`)
- post-seal score:
  `held4_bothfamilies_r1_postseal_score_20260729.json`
  (`74fe9e211c383436650863596cf15a974824ccb1397b9c72dad335e8fdabe543`)
