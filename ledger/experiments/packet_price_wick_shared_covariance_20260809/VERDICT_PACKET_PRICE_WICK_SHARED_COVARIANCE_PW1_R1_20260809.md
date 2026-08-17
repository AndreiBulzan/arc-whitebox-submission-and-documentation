# Packet Price/Wick shared covariance PW1/R1 verdict

Date: 2026-08-09

Verdict: **pass the local representation; promote orders 2 and 4 to one
complete-K129 target-free test**.

Evidence label: **component**.  This is not a benchmark score, physical row,
or estimator receipt.

On the frozen four-network/sixteen-centre full-covariance teacher, the exact
bivariate shared-covariance ceiling retained `0.971260` centre-averaged final
correction fidelity with cosine `0.986153`.  Every family passed.  Orders 2,
4, 8, 12 and 16 were numerically converged to the same result; even order one
reached `0.971233`.

This proves that averaging covariance across centres is locally sufficient,
but it also shows that omitted higher Hermite orders are not the reason the
previous all-centre R6 order-one construction failed.  The remaining question
is unusually sharp: can the very small order-2/4 tail repair the tiny global
signed K129 common mode even though it barely changes local fidelity?

Only one broad test is licensed: complete-K129 orders 2 and 4 on the sealed
Full8/Generated8 packet population.  If both remain near failed R6 order one,
close the shared-covariance Price/Wick family and do not add order, rank, or
sketches.

Artifacts:

- `runtime/artifacts/packet_price_wick_shared_covariance_pw1_r1_targetfree_20260809.npz`
- `runtime/artifacts/packet_price_wick_shared_covariance_pw1_r1_targetfree_20260809.json`

