# K125 O0-subset scout R1

Status: **killed**.

Accuracy evidence is **component**; all count and remote-score arithmetic is
a **projection**. No estimator, FlopScope session, physical row, package,
network action, upload, or submission was used.

The requested compute saving is real in projection but does not survive the
accuracy trade:

| candidate | projected saving | Synthetic512 raw ratio | Full100 raw ratio | adjusted factor range |
|---|---:|---:|---:|---:|
| uniform K125 | `3.9002B` | `1.04594` | `1.06611` | `1.0182 .. 1.0378` |
| weighted K125 | `3.9002B` | `1.05361` | `1.05269` | `1.0248 .. 1.0257` |

The uniform support removes bases `56,103,112,113`; it is the exact minimum
of the target-free endpoint-reconstruction objective over all
`C(129,4)=11,009,376` removal sets fitted on Synthetic rows `0:512`.

The stronger constant-exact spelling removes `12,34,93,125` and fits one
fixed nonnegative weight per retained basis. Its weights sum to one and
range from `0.0072112` to `0.00898476`. It reconstructs the training
population mean slightly better, but reverses on target accuracy. A separate
target-free `0:256` fit / `256:512` proxy-development ridge check selects
the uniform-weight limit, so shrinkage does not rescue the mechanism.

Validation used disjoint Synthetic rows `512:1024` and Full100. The Full
test grafts the historical subset delta onto the exact current K129 q0, so
it remains component evidence. K126 and K124 also remain adjusted-negative
on at least one family; neither offers a smaller checkpoint candidate.

Against remote `#320919`, uniform K125 projects to approximately
`1.259e-7 .. 1.284e-7`, and weighted K125 to approximately
`1.267e-7 .. 1.269e-7`. Composing `lambda=0.0075` does not change the
decision.

Authoritative receipt:
`k125_subset_scout_r1_20260729.json`.
