# Conditional reconstruction of the K146 width-200 middle, R1

Date: 2026-07-29

Evidence sought: `component` accuracy and `projection` cost only.  This is a
fast falsifier.  It performs no FlopScope row, package action, network action,
upload, or submission.

## Fixed mechanism

Use the already-open sensitivity-screen rows:

```text
Full       211, 223, 227, 239
Generated   13,  20,  24,  27
```

The control is the sealed `energy_s12_w216` prediction.  The narrower
reference is the sealed `obsvar_s12_w200` prediction.  Both are copied from
the already sealed upstream archive before any target is read.

For each source layer 12--20, retain exactly the same 200 coordinates as
`obsvar_s12_w200`.  Let `K` and `D` denote the retained and omitted positions
inside the analytic width-232 support.  Compute one target-free conditional
map

```text
ridge = 1e-4 * mean(diag(C_KK))
B = solve(C_KK + ridge I, C_KD)
```

using the current analytic post-ReLU covariance.  No ridge grid or
target-dependent choice is allowed.  Replace the next affine map by

```text
W_K' = W_K + B W_D
fold = mu_D W_D - mu_K B W_D
```

where analytic means use the incumbent `1 / sphere_er` scaling.  All other
front repair, support, late compression, readout, and arm blending operations
are unchanged.

## Cost gate

For `n=200`, `d=32`, output width 200, and nine reconstructed transitions,
the released FlopScope formulas give:

```text
solve per layer       2*n^3/3 + 2*n^2*d       7,893,333
B @ W_D per layer     n*200*(2*d-1)            2,520,000
right add + centering + ridge bookkeeping       120,000
total added, nine layers                         94,799,997
```

Attached to the preregistered width-200 projection:

```text
count projection             135,104,340,297
effective, residual held     161,391,124,877
```

This is below the hard fast-falsifier ceiling of `165B`, so the accuracy
pilot is allowed.  These are projections, not physical receipts.

## Staged gate

First capture and seal only the first two rows of each family.  Only then may
their already-open targets be scored.

Advance to the remaining two-plus-two rows only if:

- at least 70% of the positive pooled Full width-200 loss relative to the
  width-216 control is recovered;
- Generated pooled conditional/control is at most 1.02;
- no conditional/control row ratio exceeds 2.0.

The final four-plus-four report is descriptive and must include each
family's pooled ratios, recovered-loss fraction, row ratios, and the
remote-anchor adjusted projection.  A family reversal kills the mechanism.

