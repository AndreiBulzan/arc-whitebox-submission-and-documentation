# Verdict: frozen one-frame packet candidate-1 is killed

The target-free R8D capture froze candidate index 1 after its development-bank
lead and evaluated it on disjoint Full660--675 and Generated112--127 rows.
Every line used exactly one antipodal packet pair, so this was a true
same-row-count test rather than the historical two-pair packet replay.

The pooled corrected-MSE ratio against the matching canonical q0 estimator was
`1.1515914586`.  Family ratios were `1.4846161224` on Full and
`0.8856556310` on Generated; only 10 of 32 rows improved.  The bootstrap
probability of a ratio below one was `0.18755`.  All frozen gates failed.

Therefore the development-bank candidate-1 result (`0.821901`) was selection
luck.  Deterministic global one-frame packet selection is closed for this
candidate construction.  The result does not falsify the distinct two-frame
antithetic packet estimator; that mechanism must be judged by accuracy per
charged operation and by an exact shared-compute implementation.

Evidence label: **broad statistical**.  This was not a Mini100 or physical
FlopScope receipt.
