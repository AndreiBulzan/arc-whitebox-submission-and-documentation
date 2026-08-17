# K146-anchored source-residual corrector R1 — verdict

Evidence label: **component**.

## Verdict

**Hard stop. Do not tune, widen, or deploy this spelling.**

The target-free acquisition reproduced frozen K146 predictions exactly on
Full100 and Generated64. A 48-coefficient permutation/gauge-equivariant
source-transport head projected to only `0.241456B` extra operations and
appeared to clear the gate on the Full20 validation block:

```text
validation raw ratio       0.871573
validation max-row ratio   0.627687
```

Predictions were then sealed before the reciprocal evaluation targets were
opened. Both independent gates reversed decisively:

| sealed bank | baseline raw | candidate raw | ratio | max-row ratio |
|---|---:|---:|---:|---:|
| Full20 | `2.15346e-7` | `2.73900e-7` | `1.27191` | `1.71541` |
| Generated64, noise-corrected | `2.45998e-7` | `2.89911e-7` | `1.17851` | `1.25352` |

The apparent validation signal was selection noise/non-transferable
network-specific residual structure. This closes the specific class of a
small endpoint/source-moment supervised correction attached to K146 under
the available Full60 training statistic. It does not alter the frozen K146
estimator.

No FlopScope session, physical row, package, upload, submission, or remote
action occurred.

