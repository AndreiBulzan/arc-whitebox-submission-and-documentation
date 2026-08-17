# All-layer residual recurrent R1 — cheap capacity gate

Date: 2026-07-29

Evidence sought: **component**.  This is an offline learned-capacity scout,
not a FlopScope run, physical receipt, candidate, package, upload, or remote
action.

## New observable

The K32 local repair failed because each layer/output was corrected
independently.  R1 instead carries its predicted mean error through the
literal next weight matrix:

```text
e_pre[l] = e_post[l-1] @ W[l]
e_post[l] = gate[l] * e_pre[l] + newborn(features[l], messages[l])
```

The shared newborn cell sees the existing target-free K32 trajectory
features plus signed, absolute, and quadratic transports of the previous
correction.  It is trained by full free rollout, not teacher forcing.
Therefore it has information absent from the killed endpoint head, local
scalar repair, and diagonal teacher-forced closure: the complete sequence of
particle observations and the already-predicted residual transported in the
realized weight graph.

## Fixed pilot

```text
Full train       0..39
Full dev        40..47
Full held       48..55
Generated held   0..7
```

Only Full train/dev `official_alm` may select the checkpoint.  Full held and
Generated predictions are serialized before their labels are opened by the
separate scorer.

The cell is a shared `25 -> 64 -> 64 -> 1` MLP.  The 25 inputs are the
existing 22 K32 features plus the three transported correction messages.
No MLP index, seed, neuron identity, target, or cross-network lookup is an
input.

## Hard kill

Do not enlarge the acquisition unless both held families satisfy:

1. final-layer corrected/raw MSE ratio `<= 0.70`;
2. layers 24--31 corrected/raw MSE ratio `<= 0.75`;
3. at least half of held MLPs improve at the final layer; and
4. no non-finite prediction.

For a genuine multiplier-floor K32 candidate, the later promotion bar is
much harder: final raw MSE `<= 2.5e-7` in both families.  Passing only the
ratio gate licenses a production-K trajectory sidecar scout; it is not
itself a candidate.

