# K146 empirical hidden-pilot distillation R1c

Date: 2026-07-28  
Evidence: bounded iterative `component`; compute is `projection`.

## Fixed final falsification

R1b proved that one complete signed basis still interpolates its own hidden
states but does not transfer well enough to the other 145 bases. R1c makes
the one remaining same-cost correction: retain `512 + 512` signed rows but
stratify them across every basis and every coordinate direction.

For O0, the 256 antithetic coordinate pairs are partitioned across all 129
bases: bases 0--126 receive two consecutive coordinate pairs and bases
127--128 receive one each. For O1, bases 0--15 receive 15 consecutive
pairs and basis 16 receives the final 16. Thus every arm contains every
coordinate direction exactly once, always with its negative, and every
basis is represented.

The affine pilot fit, literal `129:17` mass, ridge `1e-4`, energy support,
exact high pilot, compressed graph, and readout are unchanged from R1b.
The operation count is identical.

## Fixed rows and candidates

This final mechanism check deliberately reuses the R1b 4+4 development
rows so that the sampling change is paired. It is not a new confirmation
bank and will not be promoted as broad evidence.

```text
Full       359, 367, 373, 379
Generated   97, 101, 103, 107
```

Only these new predictions are captured:

```text
pilot_energy_w216
pilot_energy_w192
pilot_energy_w176
```

The control is the hash-pinned R1b `mean_energy_w216` prediction on the
same rows.

## Hard stop

R1c advances only if `pilot_energy_w192` has pooled MSE ratio `<=1.05`
and maximum row ratio `<=1.40` separately in Full and Generated. Otherwise
the empirical-pilot lane stops. Width 216 and 176 are reported
diagnostically; no start-7 successor is allowed after a failed w192 gate.

## Boundaries

The shared benchmark lock is mandatory. Capture opens only weights and
seeds; a separate seal-verifying scorer opens targets afterward. No
physical row, FlopScope run, package, remote action, upload, submission,
or `STATUS.json` mutation is authorized.

