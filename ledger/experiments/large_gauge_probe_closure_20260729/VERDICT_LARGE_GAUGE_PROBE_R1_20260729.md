# Large gauge/probe closure R1 — fast-stage verdict

Evidence label: **component** (offline supervised development only).

## Verdict

**Kill this architecture. Do not continue the 80-step run, tune it, or open
the untouched Full800--999 / Generated128 gates.**

The deployment graph was statically plausible (projected upper count
`4,282,201,984`, below the requested `5e9` ceiling), and the two-step smoke
run was finite with exact analytic bounds. However, the actual 20-row
development free rollout failed by orders of magnitude and deteriorated as
training proceeded:

| step | teacher probability | development final raw MSE | ratio to the rollout's Gaussian anchor |
|---:|---:|---:|---:|
| 10 | 1.00 | `1.0652182449e-2` | `1.000775` |
| 20 | 1.00 | `3.7204279285e-2` | `1.000003` |
| 40 | 0.75 | `6.2566350494e-2` | `1.000000` |

The standalone gate is `<=1e-6`; by step 40 this was about **62,566× above
the gate** and carried no measurable advantage over its own Gaussian
anchor. The run was interrupted immediately after the step-40 receipt.

This closes the specific hypothesis that a larger 64-channel / 16-probe
teacher-state recurrent closure can learn a stable useful free rollout from
the available 700-row training split under this formulation. The failure is
not deploy-cost or numerical-bound feasibility; it is state rollout
identifiability/stability.

## Scope and hygiene

- Development rows used: Full780--799.
- Training pool used: Full0--699.
- Full800--999 was not evaluated.
- Generated128 was not evaluated.
- FlopScope sessions: 0.
- Physical rows: 0.
- Packages/uploads/submissions/remote actions: 0.

