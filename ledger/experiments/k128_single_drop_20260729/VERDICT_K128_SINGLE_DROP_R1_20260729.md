# K128/K127 O0 endpoint-subset scout R1

Status: **killed**.

Accuracy evidence is **component**. Count and adjusted-score arithmetic are
**projection**. No estimator, physical row, FlopScope session, package,
network action, upload, or submission was used.

The target-free rule selected removals only by reconstructing the complete
uniform 129-basis O0 endpoint mean on Synthetic1024 rows `0:512`. Its frozen
choices do not transfer:

| candidate | removed | Synthetic512 raw ratio | Full100 raw ratio | projected adjusted factors |
|---|---|---:|---:|---:|
| K128 | `126` | `0.9967865` | `1.0150183` | `0.9901784 / 1.0082894` |
| K127 | `29,121` | `1.0248310` | `1.0570477` | `1.0112430 / 1.0430326` |

K128's `0.32%` raw improvement on disjoint Synthetic512 is real at this
component scope, but reverses to a `1.50%` loss on the current-q0-grafted
Full100. K127 is worse on both. Neither passes the robust `~0.99` adjusted
factor gate, and there is no eligible same candidate that improves raw error
on both families.

The label-using oracle bounds are also small and family-specific. Synthetic's
best pair is `4,126` at raw factor `0.995803`; Full's is `20,55` at
`0.977763`. They are explicitly ineligible and disagree in identity.

The K129/K146 affine slope projects `0.975050B` saved per deletion. Branch
tails shrink `2944 -> 2688 -> 2432`, and M267 plans shrink
`4x1032 -> 4x1024 -> 4x1016`, so row padding does not null either deletion.
The whole count remains a projection because an O0-subset estimator was not
built and the complete structured front may retain fixed work.

Authoritative replay and receipt:

- `replay_k128_single_drop_r1_20260729.py`
- `k128_single_drop_r1_20260729.json`
