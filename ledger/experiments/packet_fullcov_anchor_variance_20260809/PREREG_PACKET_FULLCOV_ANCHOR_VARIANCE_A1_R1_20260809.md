# Packet full-covariance anchor variance A1/R1 preregistration

Date: 2026-08-09

Evidence class: **component capacity oracle**. No benchmark expectation target,
FlopScope session, physical benchmark row, package, upload, submission, or
remote action is authorized.

## Question

The sealed packet marginal-variance-field oracle showed that replacing every
centre's exact full-covariance marginal preactivation variance by the
per-layer centre average retains about 98% of the centre-averaged packet
correction. Attempts to estimate that field from canonical or packet sample
moments failed because the Gaussian-closure covariance dynamics are
self-consistent rather than equal to the physical conditional variance.

This experiment tests a different lawful bridge:

> Can one or a few exact, self-consistent full-covariance Gaussian anchor
> trajectories supply the shared marginal-variance schedule for all K129
> centres?

An anchor uses the supplied weights, the fixed packet `rho/tau`, and the exact
Gaussian ReLU covariance recurrence. It never sees benchmark expectations or
packet outputs. Its marginal preactivation variance is broadcast to every
canonical centre, whose mean then rolls freely through all 32 supplied
layers.

## Frozen evidence and rules

- Reuse the four networks and sixteen oriented centres in the sealed V1
  target-free component: Full 640--641 and Generated 88--89.
- Verify all source hashes and the V1 pass status.
- Test a zero-mean anchor, fixed prefixes of the eight positive centres, and
  fixed prefixes of antipodal centre pairs. Prefix sizes are `1,2,4,8`.
- Report every individual positive/negative centre and every antipodal pair
  only as diagnostics; they may not define a production selection rule.
- The fixed primary production-shaped rules are `zero_anchor`, `positive_1`,
  and `antipodal_pairs_1`. Larger prefixes establish a cost/accuracy curve.
- Score both all sixteen centres and centres excluded from the anchor set.

## Gates

A strong bridge passes if a primary rule using at most two full-covariance
anchors has, at layer 32:

- all-centre averaged correction fidelity at least 0.85;
- all-centre averaged cosine at least 0.95;
- minimum family fidelity at least 0.75;
- excluded-centre averaged fidelity at least 0.75 when exclusions exist.

A fallback passes if a fixed prefix using at most four anchors reaches the
same all-centre and family gates. Otherwise reject this anchor spelling.

A pass is still only component capacity. It licenses a broad target-free
capture and a static/physical cost audit; it is not a score claim.

