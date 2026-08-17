# Gate-state four-frame distillation — R1 verdict

Date: 2026-08-06. Evidence label: **component**.

**Kill the lane at stage F0**, per the preregistered signal gate.

```text
family      best held R^2   sign agreement   gate
full             0.0428          0.598       R^2>=0.15, sign>=0.60
generated       -0.2879          0.521       both families required
```

The instrumented q0 replay was bit-exact against the sealed July q0 cache
on all 40 networks (worst association delta 0.0), the call signature was
stable (30 calls: 4x256 + 15x232 + 10x200 + final 256), and the teacher
deltas (RMS 4.889e-4) were built purely from the sealed four-frame
caches.  The capture and probe are therefore sound; the features do not
carry the signal.

Interpretation: per-neuron gate-boundary state of the q0 propagation
(occupancy, near-zero margin mass, E|z| innovations, antithetic-pair
divergence, per-basis dispersion at the final boundary) does not predict
the four-frame-minus-q0 correction across held networks, in either
family.  This extends the controlling negatives of the basis-trajectory
student (R2, 2026-08-04) and the paired-gate innovation screens: the
complete-frame difference behaves as fresh quadrature randomness whose
predictable-from-q0 component is below the F0 floor at this population
scale.  LESSONS.md's "the useful signal emerges only after complete
frame populations are averaged" stands unrefuted by a materially new
observable class.

Boundaries: 20 networks per family is a thin population, and the fixed
probe (ridge/GBT over 598 features) is deliberately simple.  Those were
preregistered choices; per the prereg, missing the gate kills the lane
without retuning.  Reopen only with a genuinely different causal state
(for example, a second cheap paid frame as a feature source, which
changes the information budget, not the decoder), never with more
epochs, models, or feature recombinations of this capture.

Receipts:

- capture: `runtime/artifacts/k129_f0_gatestate_sketch_capture_r1_20260806.json`
- probe: `runtime/artifacts/k129_f0_gatestate_signal_probe_r1_20260806.json`
- prereg: `PREREG_GATESTATE_FOURFRAME_DISTILL_R1_20260806.md`
