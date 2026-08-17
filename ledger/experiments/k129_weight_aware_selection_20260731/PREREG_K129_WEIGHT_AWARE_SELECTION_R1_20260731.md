# K129 weight-aware compression selection R1

Date: 2026-07-31

Evidence target: **component development diagnostic** only.

The deployed K129 descendant selects late hidden coordinates by sample second
moment alone.  This screen tests whether the selection should instead price
the coordinate's transmitted contribution through its actual outgoing weight
row.

The fixed variants are:

1. `current`: production second-moment selection;
2. `final_centered`: current late selection, final selection by centered
   sample variance;
3. `final_weighted_centered`: current late selection, final selection by
   centered variance times outgoing-row squared norm;
4. `all_weighted`: late selection by second moment times outgoing-row squared
   norm, and the weighted-centered rule at the mean-restored final layer;
5. `all_weighted_centered`: centered variance times outgoing-row squared norm
   at every selected layer.

All arms use the incumbent keeps (`192` at layers 24--30 and `176` at layer
31), endpoint lambda `0.0075`, and output scale `1.000025`.  Predictions are
captured without opening targets on Full rows 0--7 and Generated rows 0--7,
then scored by a separate process.  A variant is interesting only if it
improves pooled raw MSE on both families.  No physical row, package, upload,
or remote action is authorized by this screen.

