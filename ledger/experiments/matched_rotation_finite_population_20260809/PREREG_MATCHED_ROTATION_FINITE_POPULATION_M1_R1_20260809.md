# Matched-rotation finite-population correction M1 R1

Date: 2026-08-09

## Question

The earlier paired-polar MLMC experiment learned a fixed sparse OMP support
on Full16/Generated16 and failed on official Mini100.  It did not test the
design-expectation risk of a uniform without-replacement matched-basis audit.

For canonical per-basis endpoints `q_b` and a second complete frame `r_b`,
define `d_b = r_b - q_b`.  With a uniform size-`k` basis sample `S`, test

```text
candidate = q_bar + alpha * mean_{b in S}(d_b),  0 <= alpha <= 1.
```

The sample mean is exactly unbiased for the complete frame difference.  Its
conditional output-averaged variance is available without Monte Carlo:

```text
V_k = (1-k/129)/k * sum_b ||d_b-d_bar||^2 / (128*256).
```

Thus the target-dependent expected MSE is an exact quadratic in one shared
scalar `alpha`.  This first oracle asks whether that entire observation class
has score-compatible capacity before any network, package, or physical run.

## Frozen inputs

- canonical q0 per-basis endpoints:
  `f67aff76f3696a9d142527fbacf996bcd35dc3c128e8270550eba3f1c05ab4b0`
- complete right-Gram endpoints:
  `0db24b1e0cc1efefd917dff702920d49ce55a9ae7328ac0fb8c7a5e0d34f2809`
- complete R90 polar endpoints:
  `42e151ff8d0808181ac8072defac04e4de4659ede58d38ace3d041b4b165bd7d`
- angle-B pilot endpoints:
  `8ab5808ccec2e57db1d6e3af49e013b953f66344a6a42671d02ecdadc0cd7695`
- deep-pullback pilot endpoints:
  `b85c705a0c7847b994a3a68e4e6c878a8a8c5c9a3e3957f57f16239f549358aa`
- Full target archive:
  `e8015f009d2b89eaa10cb039fc735416fc2804fd6e991f09819754235b646f50`
- Generated target archive:
  `75aa3598218107c4dbea278ae4742ad97e726694d8592b4e2f3a45e7eab04e69`

## Frozen grid and economics

- `k in {1,2,4,8,12,16,24,32,48,64,96,129}`.
- One scalar is fitted jointly across Full and Generated rows, then reported
  unchanged on each family.
- Conservative incremental cost:
  `5.0B + 0.999B*k`.
- R94 reference effective compute: `137.8297337635B`.
- R94 reference adjusted projection: `1.1146386484507326e-7`.

The right-Gram and polar frames have broad 100/128-row evidence.  Angle-B and
the deep-pullback frames are only 16/16-row component diagnostics and cannot
promote a candidate.

## Gates

Promote this mechanism only if a broad frame has, with one shared alpha:

1. projected adjusted score at most `8.0e-8` on Full and Generated
   separately for some `k <= 32`;
2. expected raw ratio at most `0.70` on both families;
3. complete-frame association error at most `2e-10`;
4. no target, row identifier, or learned support enters the sample design.

If no broad frame passes, close uniform matched-basis finite-population
correction for these frames.  This does not close a genuinely near-identity
rotation that has not been captured, nonuniform design-unbiased sampling, or
a new observable that predicts the missing contractions.

Evidence is **broad statistical** for the two broad replays, **component**
for 16/16 pilots, and **projection** for compute/adjusted score.  No remote,
upload, submission, package, physical row, or FlopScope action is authorized.

