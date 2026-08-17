# Seven-bit window-16 MLMC17 transfer screen R1

Date: 2026-08-05

Evidence sought: target-free **component** transfer capacity on the existing
4+4 Full/Generated bank. No target score, FlopScope execution, physical row,
package, upload, submission, or remote action.

## Fixed allocation rule

The preregistered seven-bit layer-2..15 screen passed every endpoint gate and
Full residual gate. Its sole miss was Generated residual ratio `0.0819638`
against the `0.08` gate with 16 dense correction bases.

For a sample-mean correction, variance contribution scales as `V/N`. The
smallest integer correction allocation restoring the original budget is

```text
ceil(16 * 0.0819638 / 0.08) = 17.
```

R1 therefore changes only the nested high-arm counts from `(14,2)` to
`(15,2)`, retaining midpoint nesting. Low counts remain `(55,9)`. Bit width,
clipping, calibration, and the fixed layer-2..15 window are unchanged. No
alternative count or arm allocation is tested.

## Transfer gate

Run target-free Full rows `0,1,2,3` and Generated rows `2,4,5,6`. For each
family require:

- mean `residual_ratio / 17 <= 0.08 / 16`;
- maximum residual ratio at most `0.12`;
- mean multilevel discrepancy MSE below `5e-7`;
- maximum multilevel discrepancy MSE below `1e-6`.

The packed arithmetic identity and affine reconstruction must remain exact.
Pass only promotes to a sealed broad-score/Mini100 economics study; it is not
a deployable candidate or score claim.

