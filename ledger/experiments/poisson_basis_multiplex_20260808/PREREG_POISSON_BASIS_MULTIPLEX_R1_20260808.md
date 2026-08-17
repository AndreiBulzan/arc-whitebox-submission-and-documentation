# Preregistration: independent-basis multiplexed Poisson correction R1

Date: 2026-08-08

Evidence class before any target is opened: **component**.

## Question

Can the already-passing signed spherical-Poisson band correction be measured
with one probe population by spending directions independently across Kerdock
bases and multiplexing the thirteen Poisson radii within every basis?

This is a capacity/variance oracle, not a deployable estimator and not a
compute receipt.  It uses literal depth-32 dense propagation on a frozen
target-free network set.  No Mini100 row, physical benchmark row, FlopScope
session, package, upload, submission, or remote service is opened.

## Why this is materially different from the killed sparse probe

The killed R3 sparse probe evaluated two global Haar-frame rows at every
radius.  Each of those two directions was reused at all 129 bases, so it had
only two genuinely distinct tangent directions per radius and the four-band
inverse amplified their coherent noise.

R1 here instead gives every one of the 33,024 Kerdock lines an independent
tangent draw.  Each basis contains a balanced allocation of all thirteen
radii.  The estimator applies the already-frozen R2 low-band linear
functional directly, rather than estimating thirteen complete population
means and then materialising a four-band state.  The matched canonical basis
endpoint is subtracted before bases are averaged.

The closest positive ancestor is the R2 broad signed-Poisson oracle, which
used 13 radii x 64 full populations and reduced broad raw MSE by 38.84%.
The closest negative ancestor is the two-direction sparse R3 probe, whose
primary teacher-MSE ratio was 2903.48.  This experiment is justified only by
the untested factor of 16,512 in genuinely distinct line directions between
R3 and one fully multiplexed population.

## Frozen mathematics

Let `l_s` be the universal linear weights induced by the sealed R2
four-band ridge inverse.  For a basis `a`, line `j`, and radius label `S_aj`,
the multiplexed smoothed term is

```text
T_a = (1/256) sum_j [ l_{S_aj} / p_{S_aj} ] f(V_aj),
```

where `p_s = n_s / 256`, and the integer counts `n_s` are obtained by the
largest-remainder allocation of the target probabilities

```text
p_s proportional to |l_s| * sigma_s.
```

Here `sigma_s` is the sealed R2 per-population standard deviation at radius
`s`, averaged over the two families, rows, and outputs.  Every radius receives
at least one line per basis.  Labels are randomly permuted within each basis.

The matched basis correction is

```text
C_a = T_a - (sum_s l_s) Q_a f.
```

For each fixed line, random labelling and the inverse-probability factor make
`C_a` unbiased for the frozen R2 linear functional.  Each tangent is produced
from an independent isotropic Gaussian vector projected into the line's
tangent space.  Conditional on its radius label, the inner product is drawn
with a shifted, stratified inverse CDF of the exact spherical-Poisson law.

## Frozen data and randomisation

Use the 40 rows from the sealed target-free gate-state teacher:

- Full rows: 7, 17, ..., 197 (20 rows)
- Generated rows: 0, 1, ..., 19 (20 rows)

Use 16 deterministic random replicates beginning at seed 2,026,080,811.
For every replicate, randomly permute the 129 basis identities.  Report
prefixes of 16, 33, 64, and all 129 bases.  The primary production-shaped
candidate is the first 33 bases of replicate zero.  No seed may be selected
after inspecting a target.

The literal canonical K129 basis endpoints are captured once and reused only
as the matched control.  The sealed complete-cloud correction is the
target-free teacher.  Challenge expectations remain unopened until the
target-free artifact and its hashes are sealed.

## Target-free fit and gates

For each basis count, fit one scalar shrinkage coefficient on the first ten
rows of each family, pooled over all 16 random replicates.  Evaluate without
refitting on the last ten rows of each family.

The `B=33` mechanism passes to post-seal scoring only if all of the following
hold on held rows:

1. primary replicate pooled teacher-MSE ratio <= 0.65;
2. primary replicate teacher correlation >= 0.60 in each family;
3. median replicate pooled teacher-MSE ratio <= 0.65;
4. at least 75% of replicates have pooled teacher-MSE ratio <= 0.80;
5. the maximum absolute shrunken correction is <= 0.01.

The broader research success target is held teacher-MSE ratio <= 0.50, which
would retain enough of the signed correction to justify integration into the
existing 33-secondary-basis budget.

If the target-free gate passes, a separate immutable post-seal scorer may
open only the already-used Full/Generated targets for these 40 rows.  It must
require at least 20% raw-MSE reduction in both families and pooled.  Passing
still authorises only a capsule-native K162/m33 integration oracle; it is not
a physical, compute, wall, Mini100, or remote claim.

## Immediate kill interpretation

Failure at `B=33` means the 38.84% Poisson-band correction is real but cannot
be measured within the existing 33-basis secondary budget by line-level
random probing.  In that event, do not retry nearby radius grids, lucky
seeds, more generic learning, or another sparse Poisson spelling.  Only an
analytic boundary/defect contraction or a quadrature preserving the same
band functional deterministically remains in this family.
