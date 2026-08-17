# K129 selective late compression R1 — verdict

Date: 2026-07-30

Decision: **kill the exact preregistered geometry and stop.**

Evidence label: **component development diagnostic** for accuracy and
**projection** for count economics. No physical row, FlopScope session,
package, upload, or remote action was performed.

## Fixed geometry

- Control: K129 O0, `lambda=0.0075`, late keeps
  `[192,192,192,192,192,192,192]`, final keep `176`, scale `1.0`.
- Candidate: late keeps `[160,160,160,160,160,160,192]`, final keep `160`,
  output scale `1.000025`.
- Rows were fixed before prediction: Full `0..7` and Generated `0..7`.
- Both arms were captured target-free, sealed, and scored by a separate
  post-seal process.
- Hard gate: candidate/control pooled raw-MSE ratio no greater than
  `1.000273` independently on each family.

## Result

| Family | Control raw MSE | Candidate raw MSE | Ratio | Rows improved |
|---|---:|---:|---:|---:|
| Full8 | `2.081413914e-7` | `6.703164368e-6` | `32.204860` | `0/8` |
| Generated8, noise-corrected | `4.089997996e-7` | `1.081697784e-5` | `26.447392` | `0/8` |

The candidate misses the gate by orders of magnitude on both independently
named families. This is not a near miss and must not be widened into a keep
grid.

## Count economics and overlap

The exact named primary-operation saving was projected as
`3,872,983,264` FLOPs:

- six late-layer contractions: `3,539,704,896`;
- final keep `176 -> 160`: `333,278,368`.

The unchanged-raw count-only projection was `1.199672585e-7`, but the measured
raw damage invalidates it completely. No R21 cleanup, historic view,
direct-destination, or rank-49 saving was re-credited.

## Integrity

- Baseline association to the frozen endpoint grid was byte/numerically exact
  on every overlapping row.
- Target-free seal SHA256:
  `16db7c305fba130b200319faab6eb240afd8dcd178391eb588f58fedf4094af5`.
- Seal manifest SHA256:
  `4e6183134f0938d716dfe11d0cd743e92ddf792724e1833864d64d15dc615254`.
- Post-seal score receipt SHA256:
  `91d88cccbc8776f025cc14648f5ea1ee68b56453ffa049ecf86aa42ff7ed5b47`.

The preregistered stop rule is now active: no further capture, integration,
physical benchmarking, packaging, or remote work on this geometry.
