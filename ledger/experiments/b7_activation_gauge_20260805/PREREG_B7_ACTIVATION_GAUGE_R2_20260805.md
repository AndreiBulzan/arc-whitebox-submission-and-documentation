# B7 activation-gauge screen R2 — preregistration

Evidence class: target-free component/projection.

R1 was not a valid exhaustive circuit screen.  It ranked the complete
`GL(2,Z)^3` ternary-unimodular orbit by uncached row sparsity, then ran the
signed-CSE synthesizer on only the first 256 triples.  The identity/live
factorization was not in that subset even though its known circuit is better
than the reported R1 winner.

R2 removes that selection error.  For every one of the 40 ternary
unimodular 2x2 bases it synthesizes every distinct factor transformation:

- U for all 40x40 `(P,Q)` pairs;
- V for all 40x40 `(Q,S)` pairs;
- W for all 40x40 `(P,S)` pairs.

The 64,000 gauge triples are then scored from those complete tables.  The
identity gauge must reproduce the live upper bounds U/V/W = 4/4/7, and the
winning tensor is checked exactly before any result is accepted.

Primary objective: minimize activation-side nodes `U + W`; secondary:
minimize `U + V + W`.  Any strict activation-side improvement is retained as
a component lead.  Promotion to a production implementation additionally
requires that the extra weight-side work does not erase the counted saving.

No MLP targets, FlopScope session, physical row, package, upload, or
submission is opened by this screen.
