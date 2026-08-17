# Higher-strength angular-design R1 verdict

Date: 2026-07-29

Verdict: **closed by a mathematical node lower bound; do not propagate an
alleged higher-strength fixed design.**

Evidence label: **component**. This is a target-free construction audit and
dimension-dependent bound. It is not broad statistical evidence, a measured
whole, a projection receipt, or a remote result.

## Headline

The production orientation-0 cloud is already almost the smallest possible
exact spherical 5-design in `R^256`.

- It contains all `129 = 256/2 + 1` real mutually unbiased Kerdock bases.
- Adding both signs gives `129 * 256 * 2 = 66,048` unit directions.
- The Delsarte--Goethals--Seidel lower bound for any spherical 5-design in
  `R^256` is `65,792` directions.
- Thus the production construction is only one 256-vector basis, or `0.389%`,
  above the universal lower bound.

A genuinely higher exact strength cannot fit:

| exact spherical strength | universal minimum nodes | versus K146's 74,752 nodes |
|---|---:|---:|
| 4 | 33,152 | fits, but is weaker than production |
| 5 | 65,792 | production already attains this class |
| 6 | 2,861,952 | `38.29x` too many |
| 7 | 5,658,112 | `75.69x` too many |

K162 has `82,944` signed directions and is still `34.50x` short of the
strength-6 lower bound. Even the deliberately over-generous **projection**
that all `144.013B` current counted work scaled linearly with directions and
the entire `272B` cap could be spent on them gives only about `141,185`
directions, still `20.27x` short. Real estimator overhead makes that upper
capacity smaller, not larger.

## Literal production proof and audit

For one orientation, write the bases as `B_0,...,B_128`.

1. Every `B_i` is orthonormal.
2. For `i != j`, every entry of `B_i B_j^T` has magnitude `1/16 =
   1/sqrt(256)`, so the bases are mutually unbiased.
3. The audit checked all `C(128,2) = 8,128` nontrivial Kerdock phase
   differences. Every unnormalised Walsh coefficient had magnitude exactly
   `16`; maximum error was zero.
4. The fourth frame potential of the `129 * 256` unoriented lines is

   `[(129)(256) + (129)(128)] / (129*256)^2`

   which equals `3 / [256(258)]`, the exact spherical fourth moment.
5. Antipodal completion makes every odd moment through degree 5 vanish.
   Therefore the literal signed support is a spherical 5-design.

As a byte-level sanity check on the float32 arrays, 40 deterministic
coordinate/random directions per orientation gave maximum relative second
moment error `1.68e-8` and maximum relative fourth moment error `3.49e-8`.
Those are representation-rounding errors around the exact construction.

## Why no propagation comparison was run

The preregistered request was to compare a **genuinely higher-strength**
equal-node construction against Kerdock. The bound proves that no such
construction exists at K146, K162, or even under an unrealistically generous
full-budget linear node projection. Propagating:

- another maximal-MUB/Kerdock orbit would compare two designs of the same
  strength and repeat the already-studied orientation question;
- a Clifford/stabilizer, Delsarte--Goethals, Reed--Muller, or orthogonal-array
  orbit of true strength 6 or greater must violate the node/budget envelope;
- a signed Hadamard/OA rule with fewer nodes integrates a Rademacher cube, not
  the uniform sphere, unless it satisfies the same spherical moments, in
  which case the same lower bound applies;
- a great-circle polygon or an arbitrary approximate design has no higher
  exact strength and was outside this foray's claimed mechanism.

Running the existing arbitrary-input Torch adapter would therefore spend
compute on a false premise. No targets were opened.

## Strategic conclusion

The first missed angular harmonic of the current complete Kerdock design is
degree 6. Uniform, weight-independent exact cubature cannot reach it under
the challenge budget. A lawful breakthrough must instead exploit structure
that invalidates the universal-design requirement: a weight-conditioned
rule, analytic contraction of selected degree-6 chaos, or a control variate
for the directed final-mean observable. Merely swapping in a "higher code"
cannot do it.

## Seals and actions

- audit source SHA-256:
  `19b5f031c2ade2dbb81a379cac0c5616080266819bd7ff5056b0ce99c426f40d`
- receipt SHA-256:
  `30b00e0c7399dbed4334bd9998cd58569d6956168d742941283196895f0fce83`
- production compact SHA-256:
  `262e634e298209f87de47cda1b4d37128c4a1f1d6fb04dd6becc68de689faf07`

Artifacts:

- `audit_maximal_mub_design_bound_r1_20260729.py`
- `maximal_mub_design_bound_r1_20260729.json`

No FlopScope session, physical/timed row, package, upload, submission, or
remote action occurred.
