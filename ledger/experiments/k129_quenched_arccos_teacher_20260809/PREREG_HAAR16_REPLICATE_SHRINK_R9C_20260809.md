# Preregistration: Haar16 replicate-correction shrink R9C

R9B showed that the frozen Haar16 arc correction has some useful direction but
an unstable amplitude.  R9C is a post-hoc development mechanism on the already
opened R9B bank; it cannot validate itself.  It tests whether the two
independent Haar16 replicas provide a lawful target-free amplitude estimate.

For each of the two frozen R9B probe/ridge cells, fit a separate arc correction
`d1` and `d2` from each Haar16 mean replica.  Let `d=(d1+d2)/2`.  Estimate

`T=max(<d1,d2>,0)` and `V=||d1-d2||^2/4`,

then return `uniform + alpha*d`, with `alpha=T/(T+V)`.  Also compute an
output-coordinate-cross-fitted version: estimate alpha on even coordinates
and apply it to odd coordinates, and conversely.  These quantities use only
the supplied weights, K129 states, and two query replicas; no expectation
target is read during fitting.

Seal predictions before the existing R9B targets are opened by a separate
scorer.  This is only a mechanism/capacity screen.  Any useful cell must be
frozen and confirmed on another disjoint bank.  No physical, Mini100, remote,
upload, or submission action is authorized.
