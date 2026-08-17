# Seven-bit window-16 R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical capacity and static core
cost **projection** only. No scoring, physical row, package, submission, or
remote action.

The prior fixed windows quantized layers 2--23 or 2--30. Plain seven-bit
packing passed every final-discrepancy gate and the Full residual gate, but
Generated residual was `0.10896` against `0.08`. Capsule search found no
seven-bit layer-2-through-15 window result.

This single follow-up quantizes exactly layers `2..15` (14 of the 22 layers in
the previous shorter window), with every other quantizer and MLMC parameter
unchanged. It is selected mechanistically to reduce accumulated quantization
noise, not from target scores. Test only Full row 0 and Generated row 2.

Promote only if residual variance ratio is at most `0.08` and multilevel
discrepancy MSE is below `5e-7` on both families. Otherwise kill without a
window sweep.

