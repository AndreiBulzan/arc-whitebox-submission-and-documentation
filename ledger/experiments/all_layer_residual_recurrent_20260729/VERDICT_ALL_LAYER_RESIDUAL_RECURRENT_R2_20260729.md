# All-layer residual recurrent R2 — hard-stop verdict

Date: 2026-07-29

Evidence label: **component**. No FlopScope session, physical row, package,
upload, submission, or remote action was used.

## Verdict

**Kill the recurrent correction family on the existing K32 summary
capture.** R2 corrected R1's only semantic defect by converting the stored
centered gate feature back to `active_fraction`. Everything else—including
seed, splits, optimizer, rollout, and gates—was identical.

The mathematically valid carry spelling failed more strongly:

```text
                              Full held    Generated held
final corrected/raw MSE          1.6974          1.2978
late-8 corrected/raw MSE         1.5584          1.2867
final rows improved              25.0%           37.5%

required final ratio            <=0.70          <=0.70
required late-8 ratio           <=0.75          <=0.75
```

Its selected checkpoint was already worse on Full dev (`1.4628` final,
`1.3511` late-8). Therefore neither more held acquisition nor another
training variant is licensed. R1 is retained only as an execution-spelling
failure; R2 is the controlling mathematical test.

This closes the shared local-summary recurrence using the frozen
32-literal-direction capture. It does not close a genuinely new capture of
the production K162/K-low structured state that retains per-basis/per-arm
trajectories, repairs, or carried gate/pair information. Reusing the same 22
aggregate channels at a larger K would not be a new observable.

Authoritative score:
`all_layer_residual_recurrent_r2_held_score_20260729.json`
(`646552ccc927732b09f776d2358b49c707a08656aee1f5ce716c68e0eb7c7447`).

