# K129 mixed64/D6 H2 pair-merge R10

This is a target-free **component** screen.  Compute remains a
**projection** until a FlopScope whole exists.

## Prior-art preflight

Queries covered `basis merge`, `trajectory merge`, `cloud coalescence`,
`barycentric basis`, `cluster basis`, `matched48`, `direction herding`, and
the D6 literal runtime.  Prior work dropped bases before the global H1/H2
repair, thinned directions, or herded checkpoint rows.  No recorded candidate
retains all 64 bases through the proved repair and then coefficient-coalesces
only similar repaired H2 trajectories before layers 2--5.

## Fixed mechanism

1. Retain the exact literal64 construction and global H1/H2 repair.
2. On the target-free training split of the existing literal64 D6 proxy,
   compute pairwise mean squared distances between slots.
3. Restrict pairs to the same frame and the same coefficient sign.  Rank by
   `MSE(proxy_i-proxy_j) * (abs(c_i)+abs(c_j))^2`; greedily take 16 disjoint
   pairs.  The remaining 32 slots are singletons, giving 48 groups.
4. For pair `(i,j)`, replace its repaired-H2 clouds by
   `(c_i h_i + c_j h_j)/(c_i+c_j)` and carry output coefficient `c_i+c_j`.
   Singletons are unchanged.
5. Propagate the 48 representatives through exact layers 2--5.  The layer-4
   snap uses group multiplicities `2/64` or `1/64` for its global seed and
   sample moments, preserving the original statistic whenever paired
   trajectories coincide.
6. Continue the 48 D6 means/variances through the unchanged diagonal closure.

Training positions are those with ordinal modulo four nonzero in each family;
held positions are modulo four zero.  Pair selection sees no challenge target.
The held screen compares against both the original sealed literal64 correction
and the complete target-free four-frame teacher.

## Gate

Continue only if, on each held family:

- merged-vs-literal correction MSE is at most `2.0e-9`; and
- teacher reconstruction MSE is no more than `1.03x` the literal64 teacher
  reconstruction MSE.

This gate is deliberately strict because the projected downstream saving is
about 25% but does not discount the unchanged front repair, frame work, or q0
anchor.  No target score, package, upload, or submission is authorized.
