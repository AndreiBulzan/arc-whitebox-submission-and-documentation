# Layer-grouped gate-source L1/R1 verdict

Status: **the exact 32-layer partition through eight selected layers is
rejected; other collective partitions remain open**.

Evidence label: **component** on fixed Full 640--641 and Generated 88--89.
No physical row, Mini100 row, package, upload, or remote action was used.

The exact Duhamel decomposition passed before any target was opened.  The
maximum discrepancy between the directly propagated packet correction and
the sum of the 32 transported layer sources was `3.535e-9`.

The useful packet signal is real:

- pooled canonical raw MSE: `2.712074e-7`;
- pooled 16-replicate packet raw MSE: `1.232557e-7`;
- pooled net transported-source norm: `0.263416`.

It is not sparse by layer.  The sum of the 32 layer-source norms is
`2.548618`, or `9.68x` the norm of their signed sum.  Equal-weight
Horvitz--Thompson sampling therefore amplifies large mutually cancelling
terms.  Even oracle norm probabilities add `6.3965e-3` pooled MSE at four
sampled layers and `3.1982e-3` at eight.  Deterministic per-row
output-oracle subset selection also fails: the best tested 16-layer rule has
pooled raw MSE `2.6784e-4`, about three orders of magnitude above the
canonical error scale.

Adjacent blocks confirm that coarsening restores cancellation only slowly.
The sums of block norms for 2, 4, 8, and 16 adjacent blocks are respectively
`0.5092`, `0.8514`, `1.2283`, and `1.7486`, versus the same `0.2634` net
correction norm.  This is evidence against cheap independent block
sampling, not a proof against every adaptive correlated block construction.

The rejection applies exactly to:

- the 32 single-layer source partition;
- independent uniform or oracle-norm sampling through `S=8`;
- deterministic equal-weight per-row and universal subset selection through
  `S=16`.

It does **not** close adaptive multi-layer groups, correlated joint block
estimators, analytic collective contractions, or non-source estimators.  A
new grouped-source proposal should first exhibit a target-free mechanism
that preserves the observed cross-layer cancellation; merely changing the
subset optimizer is not enough.

Receipts:

- `runtime/artifacts/layer_grouped_gate_source_l1_r1_targetfree_20260808.json`
- `runtime/artifacts/layer_grouped_gate_source_selection_l1_r1_targetfree_20260808.json`
- `runtime/artifacts/layer_grouped_gate_source_l1_r1_postseal_20260808.json`

