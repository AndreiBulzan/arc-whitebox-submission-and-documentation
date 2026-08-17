# Actual K32 cross-neuron endpoint-message gate R3

Date: 2026-07-28

Evidence: **component**. This is an ordinary NumPy/PyTorch capacity test,
not a FlopScope implementation, physical row, candidate, package, upload,
submission, or remote action.

## Correction of terminology

R1 used 32 literal particles and is not the production-style K32 object.
Here K32 means the frozen equal-weight support of **32 Kerdock bases**
(16 in each production orientation), or 8,192 structured rows before any
antithetic implementation details. Its held signed-final mean error is about
`2.9e-6`.

The existing target-free endpoint atlas records each basis's signed final
preactivation mean. For nonsingular `W31`, the selected layer-30 hidden
basis-mean cloud is recovered exactly from

```text
endpoint_basis = hidden_basis_mean @ W31.
```

This reconstruction and the support are the same hash-pinned objects used
by the sealed hidden-cloud denoiser R1. No target is involved.

## New hypothesis

The failed hidden denoiser predicted a correction in hidden coordinates
using local basis values, pooled support context, and the normalized
`W31 @ W31.T` hidden graph. R3 instead exposes direct output-coordinate
messages that the old model did not have:

```text
S1 = W31.T       @ H
Q1 = (W31 ** 2).T @ H
```

where `H` contains all 32 standardized hidden-basis deviations. It includes
`S1`, `Q1`, their elementwise squares, and one deterministic pair round:

```text
B  = W31 @ tanh(S1) + (W31 ** 2) @ tanh(Q1)
H2 = tanh(B) + 0.5 H
S2 = W31.T @ H2
Q2 = (W31 ** 2).T @ H2.
```

The shared output-neuron readout also sees all 32 local selected endpoint
values, support/orientation summaries, and current `W31` column invariants.
It has no output ID, MLP ID, omitted basis, or target-derived feature.

```text
feature count          235 (fixed by capture assertion)
readout                235 -> 128 -> 128 -> 1, GELU
correction             4 * endpoint_support_std * tanh(logit / 4)
optimizer              AdamW
steps                  800
batch                  32768 output records
learning rate          1e-3
weight decay           2e-4
seed                    2026072805
checkpoint selection   equal-weight mean Full-dev and Generated-dev MSE
```

All maps are equivariant to output permutations. The fixed support-channel
order is shared by every network and is part of the target-free cubature
rule.

## Frozen whole-MLP splits

Use exactly the hidden-denoiser split freeze:

```text
Full fit/dev/held        80 / 20 / 100
Generated fit/dev/held   48 / 16 / 64
```

Target-free features for every split are serialized before any R3 label is
opened. Training opens exact signed labels only for fit/dev. Held
predictions are serialized and hash-sealed before the scorer opens held
labels.

## Gate and comparison

Primary promotion gate:

```text
Full100 held corrected/raw MSE ratio       <= 0.35
Generated64 held corrected/raw MSE ratio   <= 0.35
```

Also report absolute MSE, row median/p95/max, fraction of rows improved, and
compare to the already-sealed hidden-denoiser R1:

```text
hidden-denoiser ratio     Full 1.02416     Generated 1.01037
```

Failure stops this endpoint-message spelling. Success remains component
evidence and would require a final-ReLU mapping, FlopScope cost, physical
validation, and broad score before candidate status.
