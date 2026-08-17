# K129 selective-width screen — kill

Date: 2026-07-29

Evidence: **component** target-free oracle-best accuracy screen and
**projection** padding-aware count arithmetic. No estimator prediction,
physical FlopScope row, target access, package, network action, upload, or
remote action occurred.

## Verdict

Do not implement or physically run any of the three requested profiles:

| source-aligned profile | exact primary-kernel saving | oracle-best bias / remote raw | decision |
|---|---:|---:|---|
| `224 / 208 / 200` | `0.211354B` | `1.972%` | kill |
| `216 / 208 / 200` | `3.068824B` | `8.278%` | kill |
| `216 / 200 / 200` | `3.266968B` | `11.558%` | kill |

The count column is exact for the deployed depth-2 R40 cores and the
transition residual product, mapped onto R21's measured steady count. It is
not an exact whole-estimator ledger because there is no implementation from
which to count the smaller support, crop, copy, fold, and glue deltas.

## Why the apparent opportunity disappears

The deployed R40 width quantum is 18:

```text
K129 rows              66,048 -> padded 66,096
prefix 232 -> 224      padded 234 -> 234   (zero core saving)
recurrent 216 -> 208   padded 216 -> 216   (zero core saving)
recurrent 216 -> 200   padded 216 -> 216   (zero core saving)
prefix 232 -> 216      padded 234 -> 216   (2.8574702B saving)
```

The fixed pair-packed boundary remains a `240 x 240` kernel for all three
profiles. Only the `216 -> 200` transition's natural residual product
shrinks: by `0.2113536B` at input width 208 and `0.4094976B` at width 200.

The old smooth `sum(w_in*w_out)` model projected savings of `3.812B`,
`5.197B`, and `7.508B`. It ignored these padding plateaus. Even granting that
optimistic model, adding the measured source-aligned oracle bias projects:

```text
224/208/200     1.219e-7 .. 1.224e-7
216/208/200     1.282e-7 .. 1.287e-7
216/200/200     1.299e-7 .. 1.304e-7
```

The first profile therefore misses `1.2e-7` even under the obsolete smooth
count model. The other two lose much more raw accuracy than their best-case
compute reduction can repay.

## Accuracy boundary

The screen used 12 development MLPs and an 8,192-row paired cloud. Every
profile was compared to the source-aligned production profile using the same
cloud. Keep sets were selected from the full trajectory and omitted means
were also taken from that full trajectory. This is an optimistic
oracle-support/oracle-fold component: an implementable production version
can be worse.

Mean compression-bias MSE against production:

```text
224/208/200     4.4939581e-9    max 1.7661907e-8
216/208/200     1.8863857e-8    max 6.5946288e-8
216/200/200     2.6339306e-8    max 7.0993122e-8
```

The compression-bias energy is not itself a scored raw-MSE delta because its
cross-term with the unknown estimator error is unobserved. That uncertainty
cannot support a robust checkpoint claim; it does not rescue a candidate
whose optimistic arithmetic already misses or reverses.

## Artifacts

- `screen_k129_selective_width_oracle_component_20260729.py`
- `k129_selective_width_oracle_component_r2_20260729.json`
- `audit_k129_selective_width_static_20260729.py`
- `k129_selective_width_padding_static_r1_20260729.json`

This closes mild depth-selective width contraction on the current padded
R21 execution surface. Reopen only with a different kernel whose actual
padding threshold changes, or a higher-order reconstruction observable that
invalidates the measured oracle-bias boundary.
