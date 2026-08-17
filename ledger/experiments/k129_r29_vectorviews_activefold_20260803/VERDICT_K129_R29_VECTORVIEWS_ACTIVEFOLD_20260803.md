# Verdict: kill R29 vector-view port as an R27 marginal improvement

Evidence label: **measured whole** for the exact R29 archive lifecycle and
single initialized/steady Full0 rows.  This is not broad statistical or
remote evidence.

The exact archive passed bare setup and both predictions, but the rewrite was
a production no-op on R27.  R27 and R29 have identical counted FLOPs, public
operation counts, transport-request counts, and `__getitem__` counts:

```text
                              R27                 R29
steady counted                126,405,951,682     126,405,951,682
steady requests                         29,354              29,354
steady __getitem__                       3,358               3,358
steady residual                         0.136186s            0.128666s
```

Because the entire operation/request census is identical, the observed
`0.007520s` residual difference is run noise, not the requested call-site
elimination.  It is also below the preregistered `0.008s` threshold.  The
newer R27 binding topology does not execute the K146 hot owners patched by
this wrapper.

Verdict: **kill unchanged**.  Do not port K146 R4 vector views onto R27 again.
The useful next transport target must change an operation or request count in
R27's actual active owner.

Receipt:

`runtime/artifacts/k129_r29_vectorviews_activefold_official_runner_two_prediction_r2_20260803.json`

Archive SHA-256:

`e778ae2a09462b57c91be4d8af88fed3763d7e4b16613047b5d7be59839b837a`

No upload or remote action occurred.
