# Low-K dual final heat control C5 R1 verdict

Date: 2026-08-09

Verdict: **kill the final-ReLU heat-control spelling**.

Evidence label: component development diagnostic on the fixed public16
screen.  No Mini100, physical, packaged, or remote claim is made.

The literal K20 ordinary plus K20 polar baseline had raw MSE
`8.2188125886e-7`.  Selection on the preregistered even-position discovery
half chose relative smoothing scale `4` and correction multiplier `0.25`.
That candidate had:

- discovery ratio `0.9947600` (only `0.524%` apparent improvement);
- validation ratio `1.0260503` (`2.605%` worse);
- pooled ratio `1.0091900` (`0.919%` worse);
- `4/8` validation rows improved;
- worst validation row ratio `1.16347`.

All promotion gates failed except the bounded worst-row guard.  The
matched-Gaussian end of the path was much worse, and large smoothing scales
converged back to the baseline without exposing the required 5--8% signal.

This closes only a final-boundary heat-smoothed Gaussian control.  It does
not close the exact low-K dual statistic, a deeper sparse-tail handoff, or a
cheaper physical contraction engine.

Controlling receipts:

- target-free capture:
  `runtime/artifacts/lowk_dual_final_heat_control_c5_r1_targetfree_20260809.npz`
  (`c4b6ba44a0f287e9178b0ded9c62b345900c436a6e8caaad4436423e94d926b3`);
- post-seal score:
  `runtime/artifacts/lowk_dual_final_heat_control_c5_r1_postseal_20260809.json`
  (`7b2a12b7dba824d84c49ff8d7d064f5d5cb5ba496ab49ded9a33f3b780d454ef`).
