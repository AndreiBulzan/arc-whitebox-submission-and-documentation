# Packet full-covariance anchor global A2/R1 preregistration

Date: 2026-08-09

Evidence label: **component**. This experiment uses only the already-sealed,
target-free Gaussian-packet population and supplied weights. It does not open
benchmark expectation targets and authorizes no FlopScope session, physical
row, package, upload, submission, or remote action.

## Motivation

The fixed first antipodal pair reproduced local centre responses but failed
the signed average over all 66,048 K129 centres. Before rejecting the anchor
family, measure the correct sufficient quantity directly: the complete-K129
packet correction after global cancellation.

## Frozen construction

- Networks: all eight Full and eight Generated rows in the sealed packet
  oracle.
- Candidate anchor pairs: the same eight Kerdock-spread lines frozen by the
  full-covariance micro-oracle: basis indices
  `(0,18,36,54,72,90,108,128)` and within-basis rows
  `(0,31,63,95,127,159,191,255)`, each with its antipode.
- Prefixes: `1,2,4,8` antipodal pairs, fixed in that order.
- For each prefix, average its exact self-consistent marginal preactivation
  variances at every layer, broadcast to every oriented K129 centre, propagate
  the Gaussian means, and apply the packet radial normalization.
- Validate the literal canonical cloud against the sealed q0 for every row.

## Metrics and gates

Use the 64 packet replicates to remove packet-mean Monte Carlo noise from the
correction energy and residual MSE. Report pooled and family correction
fidelity, cosine, residual MSE, and the complete prefix curve.

A prefix passes representational capacity if:

- pooled final correction fidelity is at least `0.70`;
- pooled cosine is at least `0.85`;
- each family fidelity is at least `0.60`;
- unbiased residual MSE is at most `3.1e-8`.

A prefix with at most sixteen anchors that passes licenses a broad final-MSE
capture and exact production cost accounting. If the eight-pair prefix fails
all gates, close this global shared-anchor variance family. This result does
not adjudicate centre-dependent low-rank variance fields.

