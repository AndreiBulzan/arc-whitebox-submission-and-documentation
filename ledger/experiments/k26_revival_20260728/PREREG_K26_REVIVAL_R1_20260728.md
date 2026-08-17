# Preserved K26 descendant revival R1

**Disposition (2026-07-28): superseded before execution.**  An exact
historical remote result and a current 0.9.1 measured-whole receipt were found
after this preregistration was written.  No CUDA capture or target scoring was
run under this plan.  See
`K26_REVIVAL_RECONCILIATION_20260728.md`.

Evidence class: **component**.  This is an offline CUDA accuracy adapter, not
a FlopScope execution, whole-estimator receipt, package gate, or remote
claim.

## Question

Does the exact preserved K26 direction measure and mathematical estimator
descendant have raw error plausibly at or below `1e-6` while using only 26
seeds / 13,312 propagated rows?  If yes, it is worth a separate
participant-safe lifecycle revival and lawful metering campaign.  If not,
stop before spending engineering work on packaging it.

## Frozen provenance

The following byte-exact files were copied without modification from the
canonical source closure named by
`K26_315736_CAPSULE_ARCHAEOLOGY_20260728.md`:

- `base_estimator.py`,
  expected SHA-256
  `b732bf218d9ff9b586bc2fe6a1528ed73765943532f0248e7bcae11b2fd5f805`;
- `estimator.py`,
  expected SHA-256
  `8f0e1d3be22767b28088e59057e18f0426ca6cd7079cfb2efc3e4a2fe966bd3f`;
- `spherex_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_finalonly_h1half_inputs_256x6656_module.npz`,
  expected SHA-256
  `4b5aff20598c97d1ea50ba28dc5fc972e4b16b8985aa02d4f4db05109e3a9949`.

The copied Python files are provenance only and will not be imported.  The
new adapter re-expresses their numerical equations with Torch/NumPy.

## Adapter contract

The adapter must reproduce:

1. the exact float32 `6656 x 256` direction array and float64 `26 x 1`
   learned seed weights from the preserved asset;
2. the exact 26-seed antithetic expansion;
3. H1 mean/variance repair, H2 analytic-kernel partial repair, and the
   layer-4 Edgeworth mean snap;
4. the call-aligned analytic full-covariance screen schedule:
   drop 24 output columns at product calls 5--19 and drop 56 at calls 20--29;
5. the 232 middle / 192 late / 176 final energy keeps;
6. the full-covariance final-preactivation control shift with the preserved
   coefficients;
7. the learned seed-weight/gamma final readout.

All screen and keep choices are target-blind.  The adapter may use dense CUDA
matmul in place of the original exact sorted-sparse product implementation.
Those products are algebraically identical, but float32 accumulation order
is not.  Therefore there is **no byte/numerical association claim to the
historical FlopScope implementation**.  Any favorable score authorizes a
later association study; it does not validate the preserved package.

## Sealed development screen

Before any target access, seal predictions, source hashes, row identities,
seeds, and array hashes on these existing pinned rows:

- Full: `180, 182, 183, 184, 187, 191, 194, 195`;
- Generated: `100, 102, 103, 104, 106, 111, 112, 115`.

The target-free capture owns `runtime/.benchmark_lane.lock`.  Targets may be
opened only by a separate scorer after validating the complete seal.

## Fixed decision

Advance to a broader target-free capture only if all are true:

- Full raw mean MSE is at most `1e-6`;
- Generated raw mean MSE is at most `1e-6`;
- Generated noise-corrected mean is finite and at most `1e-6`;
- no individual row exceeds `1e-5`;
- all predictions are finite.

If this gate fails, stop the K26 revival.  If it passes, create a separate
preregistration for a broad bank; do not treat this 8+8 screen as broad
statistical evidence.

No FlopScope, physical/timed row, package, network, upload, submission, or
`STATUS.json` action is authorized.
