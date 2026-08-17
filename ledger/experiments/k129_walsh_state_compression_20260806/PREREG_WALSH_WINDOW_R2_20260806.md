# K129 confined Walsh-window R2 preregistration

Evidence target: **component**.

R1 killed projection after every linear layer.  R2 tests the distinct
mechanism that a short, expensive layer window may tolerate spectral state
compression without allowing small gate errors to compound through the full
depth.

## Frozen geometry

- Rows: Full `{0,17}` and Generated `{0,17}`.
- Selector: target-free global spectral energy only; no selector sweep.
- Diagnostic rank-128 windows: layers `2..5`, `6..11`, `12..17`, `18..23`,
  and `24..31`.
- Deployment-shaped middle window: layers `12..23` at ranks `{128,160,192}`.
- All layers outside the named window use the unprojected K129 state.
- Every projected layer reconstructs all 256 direction values before its
  unchanged nonlinearity.
- Front, repairs, screening, late neuron selection, final mean restoration,
  gamma readout, and output scale remain unchanged.

## Gate

A spelling advances to disjoint 8+8 only if its pooled raw-MSE ratio to exact
K129 is at most `1.08` on both families, with neither observed row above
`1.50`.  Count and movement pricing follows only for survivors.  R2 is killed
if no method passes; do not tune layer boundaries on these four targets.

Capture opens weights only and seals predictions.  A separate scorer opens
targets afterward.  No FlopScope, package, upload, submission, or remote
action is authorized.

