# Preregistration: early-checkpoint dual gate-moment sweep E1

Date: 2026-08-09

Evidence sequence: target-free capture on a new Full4 and Generated4 block,
then post-seal **component** scoring. Compute and adjusted-score values are
**projections**. No physical FlopScope run, official Mini100 access, upload,
submission, or remote action is permitted in E1.

## Question

Checkpoint 16 retained a broad 29--31% raw improvement but cost a modeled
212.429423672B and was economically killed. E1 asks whether the same frozen
selection rule can act earlier, retaining enough gain while avoiding more of
the duplicated q0/polar prefix.

## Frozen rows

- Full: `8, 108, 208, 308`.
- Generated: `116, 117, 118, 119`.

These are disjoint from D1--D4 and the B1 broad replay. Targets remain closed
until the entire capture is written and hash-sealed.

## Frozen candidates

Checkpoints:

```text
2, 4, 6, 8, 10, 12, 14, 16
```

At each checkpoint, evaluate two candidates:

- `d3`: the unchanged `whole_state_probe_m12_p32` rule;
- `cpa_zzrr`: D3 plus the strict-upper entries of the complete 32-probe
  pre-ReLU and post-ReLU second-moment matrices.

For `cpa_zzrr`, initialize from the D3 signs and make two deterministic
coordinate-flip passes. Normalize the D3 and CPA objectives by their respective
feature counts. Accept a flip only when it improves the combined normalized
objective and leaves the original standardized D3 residual no greater than
`1.05` times the frozen D3 residual. This is the preregistered hard guard.

At each checkpoint:

1. propagate complete q0 and `polar_q0_right_d2` populations exactly;
2. evaluate the 32 coordinates of the actual next-layer ReLU whose supplied
   right-operator columns have greatest squared norm;
3. select one whole antipodal pair per Kerdock line with each of the two frozen
   rules above;
4. propagate the selected 66,048 states through one exact remaining suffix,
   including the production layer-4 snap when the checkpoint precedes it.

No feature, support count, selection optimizer, frame, or endpoint correction
may change between checkpoints. The CPA arm adds 992 off-diagonal coordinates
and no additional neural matmul.

## Projected economics

The inherited operation model for 32 probes is

```text
C(cp) = 148.429423672B + 4B * cp.
```

This is a projection, not a physical receipt.

## Selection and gate

After sealing, choose the `(checkpoint, variant)` minimizing the worse of the
two family projected adjusted scores. The CPA projection adds `0.7B` to the D3
cost; D3 adds zero beyond the inherited model. Promote only to a frozen
disjoint replication if:

1. projected adjusted score is at most `1.10e-7` in both families;
2. at least 3/4 rows improve over q0 in both families;
3. gain retention versus the complete dual teacher is positive in both;
4. all recorded selected-state/probe first-moment discrepancies are at most
   `1e-4` relative RMS.

CPA is considered mechanistically positive only if, at the same checkpoint, it
also improves raw ratio by at least `0.03` in both families or adds at least ten
percentage points of complete-dual gain retention in both families.

E1 is a target-aware checkpoint-capacity screen. A passing checkpoint must be
frozen and replicated before broad work; it is not bankable evidence.
