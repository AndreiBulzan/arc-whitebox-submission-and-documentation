# Official Mini100 R34 mixed64/D6 replay R3

Date: 2026-08-04

Replay the frozen, target-free R34 mixed64/D6/q0-core64 numerical statistic on
all official Mini100 rows. The frame, basis, coefficient, q0 support, and q0
anchor weights are exactly those in
`mixed64_d6_q0_coreset_constants_20260803.py`; nothing is selected or fitted
on Mini100.

The control is the ordinary q0 prediction returned by the same CUDA adapter.
Predictions are sealed before targets are mapped. The q0 replay must associate
to the saved exact physical R26 Full7 final layer within `5e-4` maximum
absolute error. R34 remains a component-associated CUDA proxy until a winning
Mini100 result is compared with its exact package on one physical row.

Historical whole-price evidence implies the fixed screens:

- raw ratio `<= 0.928`: projects better than remote R31;
- raw ratio `<= 0.801`: projects at or below `1e-7`;
- bootstrap raw-ratio upper endpoint must be below `1.0` for either promotion.

This test produces broad statistical raw evidence and an adjusted projection,
not a submission receipt. No upload or submission is authorized.

