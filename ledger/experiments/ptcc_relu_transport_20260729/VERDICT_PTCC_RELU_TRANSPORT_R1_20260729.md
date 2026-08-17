# PTCC ReLU-transport R1 verdict

Date: 2026-07-29

Evidence label: **component**.  This was a teacher-state one-ReLU screen on
two small Full strata, not Generated transfer, a free rollout, a measured
whole, or deployment evidence.  No FlopScope session, physical row, package,
upload, submission, or remote action occurred.

## Verdict

**Kill the derivative-plus-Gaussian-birth transport spelling.**

The inherited rank-8 tensor itself survives one nonlinear transition
surprisingly well: on the four rows its final K4 query retained correlation
`0.9971` with relative RMSE `0.0745`, comparable to refitting PTCC directly
at the next post state.  That is a representation result, not a usable
readout result.

The preregistered Gaussian ReLU birth term was neither precise nor helpful:
its two 32,768-point antithetic Sobol replicates disagreed by relative RMS
`0.82--1.35`.  Adding it worsened the sensitive Edgeworth result.

```text
stratum             exact-K3 baseline   inherited+birth   ratio
Full 0,1                1.6902e-7          2.3569e-7      1.394
Full 100,101            1.7437e-7          1.4041e-7      0.805
required                                                    <=0.750
```

The inherited-only arm also reverses by stratum (`2.2901e-7` versus
`1.3750e-7`).  High unweighted K4 correlation is again insufficient in the
small-variance, high-Edgeworth-sensitivity coordinates.

Do not enlarge Sobol support or tune a birth coefficient.  Reopening PTCC
transport requires an analytic, sensitivity-aware ReLU birth/response state
and an independently accurate K3 channel—not more samples of this Gaussian
birth or another endpoint blend.

Authoritative capture:
`ptcc_relu_transport_r1_targetfree_20260729.npz`,
SHA-256
`0b71881ee15fbc58ee259b77590c8fab572d63cc785768000c2e9f0f99d88abb`.
