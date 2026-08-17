# Preregistration: asymmetric dual, thin q0 arm R1

Date: 2026-08-05

## Question

The frozen complete dual statistic

```text
0.5 * (complete q0 + complete polar(q0,right,d2))
```

has broad raw ratios `0.5239`, `0.5287`, and `0.5430` on Full100,
Generated100, and official Mini100, but its naive physical projection is
about `26.55B` above the conservative `1.10e-7` compute ceiling.  Previous
sparse experiments reduced the polar arm.  This experiment instead keeps the
complete polar arm and replaces only the q0 term by a fixed natural prefix of
the already sealed q0 per-basis endpoints.

## Frozen candidates

- q0 prefix sizes: `96`, `100`, `104` in the existing immutable endpoint
  order;
- equal weights within the prefix;
- complete polar arm `polar_q0_right_d2`;
- output weight exactly `0.5 / 0.5`;
- no fitting, target-dependent selection, row routing, or coefficient scan.

The candidates are fully determined by the two pre-existing target-free
Mini100 seals.  The scoring program may open Mini100 targets only after
constructing all three predictions.

## Fast gates

Promote only if at least one frozen size:

1. has Mini100 raw MSE no greater than `1.55e-7`;
2. improves at least `80/100` rows relative to complete q0; and
3. has a conservative linear price projection no greater than `246.95B`.

The price projection is only a screen.  A passing statistic still needs a
capsule-native implementation and a measured-whole current-meter receipt.

