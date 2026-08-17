# K146 checkpoint direction thinning R1

Evidence class: **component**.  This is an offline, target-free numerical
capacity screen.  It is not a physical FlopScope row, a package gate, or a
remote claim.

## Question

Can the current K146/M17 cloud discard direction rows after the cloud has
already carried the nonlinear early network, while retaining the accuracy of
the sealed dense chassis and reducing the row-proportional tail count by
20--30%?

## Frozen chassis and rows

- Numerical chassis: the capsule-native K146/M17 `energy_s12_w216` graph used
  by R17.
- Association control: the sealed Full0 K146 GPU receipt
  `k146_m17_c21_direct_output_gpu_a0_full0_r3_20260728.npz`.
- Development bank, opened target-free first:
  - Full: rows 211, 223, 227, 239.
  - Generated: rows 13, 20, 24, 27.
- Targets are forbidden until every candidate prediction, source hash, row
  identity, seed, and array hash has been sealed.

## Fixed candidates

At the output of checkpoint layer 12, and separately at the output of layer
23, retain `m` direction rows per sign and per basis for
`m in {192, 160, 128}`.  Positive and negative signs retain the identical
direction IDs within each basis, preserving antithetic pairs.  The direction
set varies deterministically by basis:

```text
direction(b, j) = (73 * (b*m + j) + 19) mod 256
```

where global basis index `b` runs continuously through O0 and O1.  Since 73
is odd, this is a permutation cycle.  Thus every basis has exactly `m`
distinct directions, while every direction ID occurs either floor or ceil of
the ideal incidence globally and within each arm.  The schedule is shared
between signs and retains O0/O1 in their original basis proportions.

Only selected rows are propagated after the checkpoint.  Readout uses the
arithmetic mean over the retained directions inside each sign/basis, the
usual positive/negative half sum, and the existing equal basis/arm weights.
There is no renormalization fitted to targets.

## Gates fixed before target access

1. The dense control must associate to the sealed Full0 reference with
   relative RMSE at most `2e-6` and maximum absolute error at most `3.2e-5`.
2. A fixed-subset candidate is called **near-neutral** only if, on both
   families:
   - pooled raw-MSE ratio to dense control is at most `1.10`;
   - maximum per-row ratio is at most `2.0`; and
   - at least two of four rows improve or tie.
3. It is called an **advance** only if it is near-neutral and its projected
   steady counted-FLOP reduction is at least 20%.
4. If no fixed candidate is near-neutral, stop this axis.  If at least one is
   near-neutral, only one additional target-blind checkpoint coreset may be
   tested: at the winning checkpoint and cardinality, select a deterministic
   balanced subset minimizing mismatch of the checkpoint cloud's first two
   moments, without reading targets.  That follow-up requires a separate
   preregistration and prediction seal.

Count figures from algebra or operation-ledger decomposition remain
**projection** only.  No timed physical rows are authorized by this screen.

