# Packet Price/Wick broad PW2/R1 verdict

Date: 2026-08-09

Verdict: **close the complete shared-covariance Price/Wick family**.

Evidence label: **component**, target-free.  No expectation target, physical
row, FlopScope session, package, upload, submission, or remote action occurred.

On the complete 66,048-centre Full8/Generated8 packet population:

| order | final fidelity | cosine |
|---:|---:|---:|
| 2 | `-66.788821` | `0.112587` |
| 4 | `-66.793537` | `0.112580` |

The order-four minus order-two final prediction RMS is only `0.006524` of
the packet-correction RMS.  Thus the Price series is converged in precisely
the regime where the global signed correction is missed.  Q0 association and
covariance diagnostics passed.

PW1's 97.1% local fidelity was real, but it does not survive the cancellation
over all K129 centres.  The failure is the centre--covariance/common-mode
dependence discarded by averaging covariance, not missing Hermite order.
Higher order, more coefficient Grams, or sketches of this same shared state
cannot fix it and are closed.

Artifacts:

- `runtime/artifacts/packet_price_wick_broad_pw2_r1_targetfree_20260809.npz`
- `runtime/artifacts/packet_price_wick_broad_pw2_r1_targetfree_20260809.json`

