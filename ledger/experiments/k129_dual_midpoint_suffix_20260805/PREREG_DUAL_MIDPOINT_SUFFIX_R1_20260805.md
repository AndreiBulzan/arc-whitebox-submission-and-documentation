# Dual-frame midpoint shared suffix R1

Date: 2026-08-05

Evidence sought: target-free **component** capture followed by a small
two-family accuracy falsifier.  Compute and adjusted-score values are
**projections** until an exact FlopScope graph exists.  No package, upload,
submission, or remote action is authorized.

## Prior-art boundary

Blocking searches covered `shared suffix`, `suffix sharing`, `cloud
midpoint`, `average cloud`, `late merge`, `branch merge`, `dual suffix`,
`trajectory coalescence`, and the existing handoff/checkpoint verdicts.
Existing work compresses particle states, replaces suffixes by Gaussian or
mean closures, merges coefficients inside the mixed-D6 sidecar, or samples a
sparse second frame.  No recorded candidate propagates two complete frames
to a fixed hidden layer, averages their *paired full particle clouds*, and
then evaluates one ordinary nonlinear suffix.

This therefore tests a materially different mechanism.  It does not reopen
the killed low-rank paired-delta or Gaussian-handoff spellings: no state is
projected, moment-matched, resampled, or analytically closed before the
merge.

## Mechanism

The broadly confirmed two-frame estimate is

```text
y_dual = 0.5 * (y_q0 + y_polar).
```

The q0 and polar designs have the same labelled Kerdock basis/phase rows.
Let `h0_l` and `hp_l` be their paired post-ReLU clouds.  For a fixed layer
`l`, R1 forms

```text
h_mid_l = 0.5 * (h0_l + hp_l)
y_l     = ordinary_q0_readout(ordinary_suffix(h_mid_l)).
```

If the two trajectories stay in the same ReLU cells in the suffix, this is
exactly the midpoint of the two complete outputs.  Deep ReLU correlation
makes late suffixes the favorable regime.  Economically, every layer after
the merge is evaluated once rather than twice.

## Frozen pilot

- Full rows: `7, 17, 27, 37`.
- Generated rows: `0, 1, 2, 3`.
- Complete frames: ordinary q0 and the already frozen
  `polar(q0 + right + d2)` frame.
- Merge checkpoints, post-ReLU and zero-based: `{16, 20, 22, 24}`.
- Endpoint lambda: `0.0075`; output scale: `1.000025`.
- The capture must reproduce the independently sealed complete q0 and polar
  outputs on all eight rows before targets are opened.

For a deliberately conservative first economic screen, model

```text
C_l = 273.5B - 4.0B * (31-l) + 1.0B.
```

The `4.0B` per shared layer is below the recurrent one-arm layer cost in the
existing graph; the `1.0B` term charges the midpoint and lifecycle overhead.
This model selects a hypothesis only.  It may not support a deployment claim.

After all predictions are sealed, choose the checkpoint minimizing the worse
of the Full/Generated projected adjusted scores, with ties favoring the later
merge.  Promote to a fixed official-Mini100 capture only if both families:

1. project to at most `1.10e-7` from remote R31;
2. have raw-MSE ratio to q0 at most `0.65`; and
3. improve at least three of four rows.

Failure kills midpoint suffix sharing at these economically relevant depths.
Success freezes exactly one checkpoint before Mini100.  A Mini100 pass must
still be followed by an exact current-meter graph and a measured whole.

