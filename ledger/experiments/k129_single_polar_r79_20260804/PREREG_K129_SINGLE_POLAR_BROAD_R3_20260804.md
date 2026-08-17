# K129 single-polar selection-disjoint broad confirmation R3

The candidate is frozen as `polar(q0 + right_gram + jacobian_d2)` with equal
weights.  No coefficient, shrinkage, row selector, or router is fitted here.
The official Mini100 result already exists and is not reopened during target-
free capture.

Capture exactly two complete K129 trajectories on each of two rows sets that
were not used to select the polar candidate:

- Full1000 rows `200:300`;
- Generated128 rows `16:116`.

The two methods are unchanged q0 and the fixed polar frame.  Seal predictions
before opening the corresponding targets.  Promote as a likely `<=1.1e-7`
accuracy mechanism only if:

- both family mean raw-MSE ratios are at most `0.94`;
- both family bootstrap 95% ratio intervals have upper endpoint below `1`;
- at least half the rows improve in each family; and
- the already sealed official Mini100 central ratio remains at most `0.94`.

This is broad statistical evidence for transfer, not remote evidence.  The
separate physical source gate controls count, wall, lifecycle, and numerical
association.
