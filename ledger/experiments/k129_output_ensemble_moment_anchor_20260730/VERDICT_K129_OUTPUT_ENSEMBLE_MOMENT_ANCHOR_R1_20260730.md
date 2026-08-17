# K129 output-ensemble moment anchor R1 — target-free hard kill

Date: 2026-07-30.  
Evidence label: **component**.  
Decision: **hard killed before sealing or opening a target**.

The proposed statistic is distinct from the previously killed endpoint
common-mode features: it computes the exact fresh-He-output expectation

```text
A = sphere_er * mean_node(||h30||) / sqrt(pi * 256)
```

from the actual K129/R21 final-hidden cloud. The fixed, no-fit candidate was

```text
q_anchor = q + (A - mean(q))       # beta = 1
```

It failed the preregistered output-validity gate on the third fixed Full row:

```text
Full index                         153
A                                 0.504915842412
mean(q)                           0.534343298512
global shift                     -0.0294274561003
minimum q                         0
minimum q_anchor                 -0.0294274561003
```

The anchor is about `5.5%` below the current output mean on that MLP, and the
resulting constant shift turns a valid nonnegative ReLU-mean coordinate
negative. This is not roundoff. Because nonnegativity was fixed in the
preregistration, clipping or shrinking `beta` after observing the failure
would define a new, selected estimator. Neither is allowed.

No NPZ seal was created, no target or label-noise member was opened, and no
score was computed. There was no FlopScope session, physical row, package,
upload, submission, or remote action. Do not score, tune, broaden, implement,
or package this mechanism.

The older
`k129_exact_final_residual_20260730` seal is not a replacement result. It is
the same mathematical estimator as the already closed exact omitted-final-24
product; its `192+8` spelling only reduces execution cost. The existing broad
ratios (`0.990546` Full and `1.001058` Generated) already kill its accuracy
claim, so reopening targets for its tiny seal would only duplicate a known
family reversal.
