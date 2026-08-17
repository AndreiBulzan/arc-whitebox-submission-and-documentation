# Verdict: dense quenched arc-cosine teacher passes; production quadrature fails

Date: 2026-08-09

Evidence label: **component**.  This is a four-row sealed post-capture diagnostic
(two Full and two Generated rows), not a Mini100 or physical receipt.

## Result

The high-precision target-feature teacher is a decisive representation pass.
Using 4,194,304 antithetic generic target rows, the best frozen ridge cell
(`relative=1e-6`) changed corrected MSE from

`1.9674367650740767e-7` to `1.4494080766636972e-8`,

a ratio of `0.07366986844983173` (92.63% raw reduction).  Family ratios were
`0.0801810` on Full and `0.0627230` on Generated.  Every captured row improved;
the four row ratios were `0.06646`, `0.16562`, `0.02621`, and `0.08584`.

The production-shaped Haar-frame quadrature did not recover the target kernel
mean.  Its best target-open diagnostic used eight complete frames (4,096 rows)
and `relative=0.03`, with pooled ratio `0.9647951`.  It improved only 3.52%
overall, reversed on individual rows, and retained far below the preregistered
70% fraction of dense gain.

## Interpretation

This does **not** kill the exact-layer-31 population teacher.  It establishes
that the teacher contains unusually strong target-relevant information and
that R1's weak 65,536-row result was target-feature Monte Carlo noise.  It also
kills ordinary complete-Haar input cubature through 4,096 rows as the bridge.

The next research object is now sharply isolated: compute or estimate the
target-side one-layer arc-cosine kernel mean with much lower variance than
ordinary input sampling.  Candidate representation, final-row transfer, and
the shared 129-weight readout are no longer the controlling uncertainties.

## Pinned artifacts

- target-free R2 capture SHA-256: `52b46a64d413e7c6685f8942c350ad17b92e6b0ee80265801e3630b864921827`
- target-free prediction SHA-256: `0fd9b3169ed963f6d925cd1a997935af98fe06cdeb2494f2ac51f9c55b1a85f8`
- sealed score SHA-256: `453fe3450f4b7bbeaed19ba04e75cdc4bcdf4311c777492e7caf5d847bc1858e`

No physical row, official Mini100 row, FlopScope session, upload, submission,
or remote action was performed.
