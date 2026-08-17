# Terminal teacher-forced paired-secant QDEIM R1/R2 verdict

Date: 2026-08-07

## Verdict

**Kill the rank-16 / q=20 Canonical-Anchored Paired-Secant QDEIM shadow.  Do
not build the 32-layer reduced rollout, audit correction, local bases, or
memory closure.**

The strongest necessary-condition oracle failed before rollout.  It supplied
the exact layer-31 packet secant for every candidate, learned the basis from
512 exact distinct-line pilots, applied the actual full-width final weight and
all 512 final branch ReLUs, and substituted exact pilot outputs.  Even this
overpowered spelling did not approximate the finite-pool packet correction.

No benchmark target, FlopScope session, physical row, estimator edit, score
projection, package, upload, or submission was used.

## Results

Held paired-secant state energy captured by ordinary POD was broad:

| Rank | Mean held state energy captured |
|---:|---:|
| 8 | `54.89%` |
| 12 | `62.85%` |
| 16 | `68.30%` |
| 20 | `72.54%` |
| 24 | `75.86%` |

The output-aware hybrid rank-16 basis captured `67.65%`.  More importantly,
the actual final packet-correction fidelity was catastrophic:

| Terminal reconstruction | Pooled correction fidelity | Full | Generated |
|---|---:|---:|---:|
| POD-16, exact coefficients | `-25.87` | `-26.68` | `-25.29` |
| hybrid-16, exact coefficients | `-22.62` | `-22.41` | `-22.76` |
| hybrid-16, exact 20 QDEIM samples | `-195.30` | `-214.20` | `-181.68` |

The frozen gate required pooled `>=0.85`, each family `>=0.80`, and at least
12/16 rows `>=0.70`.  Q20 passed `0/16`; its best row was `-44.56`.

This was not a pivot-conditioning failure.  The median and maximum condition
numbers of the oversampled selected basis were only `4.32` and `5.60`.
Rather, discarding roughly one third of the paired secant state produces a
small per-candidate approximation whose *population bias* is enormous
relative to the delicate packet correction.  Mean hybrid exact-projection
pool MSE was `2.1490e-6`, while mean squared packet correction was only
`9.0994e-8`.

## Independent implementation audit

Because the negative fidelities were surprising, a separate Full640
association audit replaced the reduced basis by the complete 512-dimensional
identity.  The same anchor, block-weight orientation, full final layer, and
pool reducer reproduced the exact candidate pool to:

```text
maximum absolute delta    3.4122e-9
pool MSE                  3.4412e-19
```

The failure is therefore representational, not an orientation, scaling,
anchor, or decoder bug.

## Controlling interpretation

The external proposal correctly escaped the static-depth objection by
injecting exact canonical margins at every layer.  Its remaining assumption
was that packet secants form a useful rank-16--20 manifold.  The terminal
oracle rejects that assumption under the quantity that matters.

Increasing the state width is not a routine follow-up.  Even rank 24 captures
only `75.9%` of held state energy, while q=20 is the mechanism's compute
budget.  Local bases and memory were explicitly conditional on successful
terminal teacher forcing or a teacher-forced/free-rollout gap; neither
condition occurred.  The proposed 128/256-pair audit would be correcting a
shadow error orders of magnitude too large and cannot restore the advertised
budget.

The integral-preserving packet target and one-frame output-oracle existence
remain valid.  This particular low-rank hyper-reduced bridge does not.

## Receipts

- `PREREG_TERMINAL_TEACHER_FORCED_QDEIM_R1_20260807.md`
- `capture_terminal_teacher_forced_qdeim_r1_targetfree_20260807.py`
- `runtime/artifacts/terminal_teacher_forced_qdeim_r1_targetfree_20260807.npz`
- `runtime/artifacts/terminal_teacher_forced_qdeim_r1_targetfree_20260807.json`
- `audit_terminal_identity_association_r2_component_20260807.py`
- `runtime/artifacts/terminal_qdeim_identity_association_r2_component_20260807.json`

Evidence label: target-free **component** on reused Full8/Generated8, plus a
one-network component identity audit.
