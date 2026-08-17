# Local antipodal packet-GP L1/L2 R1 verdict

Date: 2026-08-08

Evidence label: **component** on fixed Full640--641 and Generated88--89.

## Verdict

Reject the frozen two-anchor isotropic local-GP correction, both as a direct
analytic estimator and as the target-free selector for the signed `S=16`
complete-basis mechanism.

Do **not** interpret this as another generic KLPQ failure.  The local model
actually predicts the large basis-source field surprisingly well.  Its
failure exposes a sharper precision barrier: the desired packet correction
is a near cancellation of two much larger terms, and the model misses the
tiny aggregate/common mode that survives that cancellation.

## What passed

The capture is mechanically clean:

- final ReLU values reconstructed from captured preactivations with maximum
  absolute error `0` on the sealed arrays;
- order-8 versus order-12 packet-geometry quadrature differed by at most
  `3.0095e-8`;
- no targets were opened during capture or source-fidelity evaluation.

For the preregistered `basis_energy` variant, flattened basis-source fidelity
was:

| Slice | source cosine | source R2 | summed-source cosine | summed-source R2 |
|---|---:|---:|---:|---:|
| Full2 | `0.95158` | `0.88587` | `0.96903` | `0.91827` |
| Generated2 | `0.96801` | `0.91585` | `0.97817` | `0.93491` |

The `global_energy` variant was marginally stronger and the uncalibrated
theory amplitude also passed the original source-field gate.  This is real
evidence that two exact antipodal final preactivations determine most of a
nearby packet response.

## Why that was still insufficient

The original fidelity gate measured the source sum before its deterministic
radial counterpart was added.  On the two families:

| Quantity | Full2 RMS | Generated2 RMS |
|---|---:|---:|
| packet source sum | `1.5251e-2` | `1.8433e-2` |
| deterministic radial term | `1.5151e-2` | `1.8489e-2` |
| surviving net packet correction | `3.0598e-4` | `2.0615e-4` |

The `basis_energy` source-prediction residual RMS is `4.5346e-3`, roughly
15--22 times the entire surviving correction.  Thus 89--92% source-energy
capture is nowhere near the roughly four-decimal common-mode accuracy this
subtraction requires.

The frozen direct post-seal score confirms the consequence:

- canonical pooled raw MSE: `2.71207e-7`;
- exact packet oracle pooled raw MSE: `1.13801e-7` (`58.04%` reduction);
- direct local-GP pooled raw MSE: `2.04536e-5` for the primary variant;
- all four rows worsen, so the primary and every diagnostic variant fail.

This is a decisive rejection of the direct spelling, not a borderline miss.

## Signed `S=16` fallback

The predicted-space optimizer was also tested under its own target-free
preregistration.  It fit each predicted total to `5.52e-10--8.52e-10` MSE,
yet the same frozen supports and weights had exact expected-source residuals
of `2.09e-7--5.88e-7`:

- pooled expected-source residual MSE: `4.21982e-7`;
- Full: `4.45495e-7`;
- Generated: `3.98469e-7`;
- individual-replicate mean residual MSE: `8.38874e-7`;
- ideal-correction R2 was negative on every row (`-1.41` to `-9.05`).

The pooled miss is about `13.6x` the deliberately loose `3.1e-8` gate and
about `48.6x` the earlier `8.68e-9` 70%-gain allowance.  Better swap search
cannot repair a predictor/teacher geometry mismatch of this size: the frozen
optimizer already fits its own predicted target nearly exactly.

## Scientific consequence

The unavailable-output exact-source `S=16` capacity result remains valid.
What is now rejected is using a two-anchor isotropic latent GP to recover its
support/weights or its summed correction.

Any continuation must compute the missing common mode directly.  A useful
proposal should explain how all Kerdock observations or actual gate-boundary
data determine the residual at approximately `1e-4` output scale.  The most
focused mathematical question is whether conditioning the local packet
field on association-scheme sector averages, or an exact Price/Wick boundary
trace, supplies that common mode without a new full propagation frame.
Another low-rank feature map, output sketch, subset optimizer, or amplitude
fit does not address the observed obstruction.

No Mini100, physical benchmark row, FlopScope session, package, upload, or
remote action was used.

## Evidence

- target-free capture:
  `runtime/artifacts/local_antipodal_packet_gp_l1_r1_targetfree_20260808.json`
- target-free fidelity:
  `runtime/artifacts/local_antipodal_packet_gp_fidelity_l1_r1_targetfree_20260808.json`
- direct post-seal score:
  `runtime/artifacts/local_antipodal_packet_gp_direct_r1_postseal_20260808.json`
- target-free predicted S16 fallback:
  `runtime/artifacts/local_antipodal_packet_s16_l2_r1_targetfree_20260808.json`

SHA-256 values are pinned in those receipts.
