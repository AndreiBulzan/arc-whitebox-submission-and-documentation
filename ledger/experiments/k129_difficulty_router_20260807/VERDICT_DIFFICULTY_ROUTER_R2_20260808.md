# Difficulty-router program — R2 verdict

Date: 2026-08-08. Evidence label: **broad statistical** (held challenge
families) over the preregistered bounded encoder run.

**Kill the per-row floor-routing lane at the continue-scaling gate.**

```text
                                     required        observed
pooled best held gain corr           >=0.30          0.3006 (graze)
both families positive gain corr     yes             yes (0.27 / 0.33)
any (m,p) cell improving BOTH        yes             NONE
held raw_full corr (encoder)         —               0.408 / 0.486
in-family (same-generator) gain corr —               0.08–0.23
```

Every cell of the routing frontier worsens the score on both held
families (best: Full100 m24@10% ratio 1.0295; Generated128 m48@10%
ratio 1.0096; kept-row ratios 3.0–9.0 against break-evens 2.6–5.1).
The encoder did not beat the handcrafted features on raw_full
(0.41/0.49 vs 0.51/0.55) and, decisively, the per-net signed gain is
barely predictable even in-family on the clean self-generated labels
(0.08–0.23). This is an information statement, not a capacity or
transfer statement: which arm wins a given row is governed by
quadrature-error structure that layerwise weight summaries do not
expose at any tested granularity, matching the eight failed selector
families recorded in
`runtime/artifacts/k129_perrow_floor_routing_selector_requirement_r2_20260807.json`.

Scaling is not justified: an in-family ceiling of ~0.23 at 625 clean
training networks extrapolates far below the 0.6 deployment bar at any
achievable corpus size. The remaining 3,250-network generation is
cancelled.

What stands: the mechanism's capacity (oracle routing 0.92–1.05e-7)
and lawfulness (per-row multiplier floor, rule text) are real and
recorded. Reopen the lane only with a materially different gain
information source — observed cross-frame data (which costs an arm), a
theoretical decomposition of per-row frame error, or an error predictor
validated >=0.6 from some future program — never with more scale of
weight-summary learners or feature recombinations.

Receipts:
- runtime/artifacts/k129_router_encoder_bounded_r2_20260808.json (gate-derived status, pinned hashes)
- runtime/artifacts/k129_router_labels_tranche1_test500_r1_20260807.json
- PREREG_DIFFICULTY_ROUTER_R1_20260807.md + PREREG_ADDENDUM_R2_20260808.md
