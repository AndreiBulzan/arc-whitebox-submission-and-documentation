# Final-weight spectral shrink R1 verdict

Date: 2026-07-29  
Evidence label: **component**  
Decision: **hard kill**

The frozen K146 correction `d = prediction - Gaussian endpoint proxy` was
projected into the canonical right-eigenbasis of each MLP's `W31`.  Three
preregistered one-parameter shrink shapes used only final singular values
and the reconstructed `q0-q1` disagreement at inference.  Full positions
`0:60` fitted the scalar, `60:80` selected the shape, and the Full20 test and
Generated64 targets remained unopened until both candidate arrays were
sealed.

The selected high-singular-value ridge shape had `eta=0.0133537`.  Its
development improvement was only `0.0403%` and it reversed on both holdouts:

```text
                                      candidate / K146
untouched Full20 raw MSE                   1.00072779
Generated64 observed MSE                   1.00031901
Generated64 noise-corrected MSE            1.00038486
required on Full and corrected Generated   <=0.90
```

Full fixed halves were `0.99934 / 1.00199`; Generated observed halves were
`1.00044 / 1.00019`.  This is not a near miss.  The final-weight spectrum
and arm-disagreement proxy do not expose a stable denoisable K146 error
component under these low-parameter Wiener/ridge spellings.  Do not broaden
the ridge/rank grid.

This closes only the three preregistered final-weight right-eigenbasis
shapes.  It does not close an observable-specific upstream connected-chaos
correction.

Authoritative artifacts:

- preregistration SHA-256:
  `a097e264db42d989f69c01881ff532014e392f688249ad1ca6548e260ecf1272`;
- sealed prediction archive SHA-256:
  `cd382a160cacc20552fdcf4ed372d25195243d715adfea547b96e38e741fd6ef`;
- selection receipt SHA-256:
  `7d1c854980b1e87f273afb3bff5ceae4675cc1e90c3e342502e797f0df3861f2`;
- post-seal score SHA-256:
  `4690d1cd03ebdebe5685fd0ba442935a1820c19cdc79f28e6cd52677212eac2b`.

No GPU, FlopScope session, physical row, package, upload, submission, or
remote action was performed.

