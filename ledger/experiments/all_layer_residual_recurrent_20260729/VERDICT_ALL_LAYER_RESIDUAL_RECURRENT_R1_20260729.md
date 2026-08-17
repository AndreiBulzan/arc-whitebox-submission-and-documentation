# All-layer residual recurrent R1 — hard-stop verdict

Date: 2026-07-29

Evidence label: **component**. No FlopScope session, physical row, package,
upload, submission, or remote action was used.

## Verdict

**Kill the exact R1 spelling; do not enlarge its 40/8 training and 8/8 held
pilot.** Carrying the predicted correction through every realized weight
matrix did not reveal transferable residual signal:

```text
                              Full held    Generated held
final corrected/raw MSE          1.1900          1.0793
late-8 corrected/raw MSE         1.1330          1.0602
final rows improved              12.5%           37.5%

required final ratio            <=0.70          <=0.70
required late-8 ratio           <=0.75          <=0.75
```

The selected checkpoint was already worse than the raw cloud on Full dev
(`1.1305` final, `1.0423` late-8), so acquisition size is not the immediate
limitation. The exact shared `25 -> 64 -> 64 -> 1` cell—22 captured K32
local cloud summaries plus signed, absolute, and quadratic transports of
the previous predicted correction—is closed.

One implementation boundary must remain explicit: capture feature 5 stores
`active_fraction - 0.5`, while the executed rollout clamps that value
directly as its carry gate. Thus this receipt kills the executed R1, not
every correctly parameterized gate-linearized recurrence.

## What remains genuinely different

The reused capture is a 32-literal-direction diagnostic cloud summarized
into 22 local channels. It is not the current production K162 structured
quadrature state, nor even a production-style K32-basis cloud. A future
production-state scout would be a new observable only if it retains actual
per-basis/per-arm layer trajectories, repairs, gate/covariance information,
or other carried pair state. Merely applying this same aggregate cell at a
larger K is not a justified reopening.

Authoritative result:
`all_layer_residual_recurrent_r1_held_score_20260729.json`.

