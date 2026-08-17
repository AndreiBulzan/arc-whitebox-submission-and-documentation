# Preregistration: matched packet coupling R1

Date frozen: 2026-08-08

## Blocking question

Can a one-frame Gaussian-packet rule recover at least 70% of the already
measured packet gain when an unavailable teacher is allowed to pair Kerdock
lines whose responses to common packet treatments are similar?

This is a necessary-condition oracle for cross-centre antithetic coupling.
It is not a contest estimator: the teacher sees final-layer responses to
eight extra common-treatment frames.

## Fixed rows and packet construction

The oracle reuses rows whose packet target is already sealed:

```text
Full1000:     640, 641
Generated128: 88, 89
epsilon:      0.20
rho:          cos(epsilon)
tau:          sin(epsilon)/sqrt(256)
```

For each unoriented Kerdock line with stored representative `u`, a candidate
response is the antipodal pair average at

```text
rho*u + tau*z, -(rho*u + tau*z),  z ~ N(0,I_256).
```

Every production-shaped arm evaluates exactly one such pair per line, hence
66,048 rows.  Coupled arms alter only the joint law of the noises; every
line's marginal noise remains exactly standard Gaussian.

## Frozen response teacher

For each supplied network, draw four common Gaussian directions.  Evaluate
both `z` and `-z` on every line.  Project the 256 final outputs through one
fixed 16-column orthonormal random projection.  The matching feature contains
the four odd responses and the four treatment-centred even responses.

Line orientation is canonicalized from the largest-magnitude odd feature.
The two teacher pairings are:

1. `teacher_basis`: greedy minimum response distance independently inside
   each original 256-line Kerdock basis;
2. `teacher_global`: minibatch k-means into 512 response clusters, greedy
   pairing inside clusters, then greedy pairing of cluster leftovers.

The common-treatment features and the evaluation noises use disjoint seeds.
Thus matching is evaluated out of treatment sample.

## Fixed arms and repetitions

Eight independent evaluation repetitions are run for each arm:

```text
independent          one independent Gaussian noise per line
random_pair          random line pairs, Z and -Z, fixed stored orientations
natural_basis        consecutive pairs inside every Kerdock basis
teacher_basis        response-matched within-basis pairs and teacher orientations
teacher_global       response-matched global pairs and teacher orientations
```

For every paired arm, one Gaussian vector is drawn per pair and assigned as
`Z,-Z`.  Marginal exactness is therefore algebraic, not empirical.

## Sealing and gates

The capture reads weights and the previously sealed target-free packet
predictions only.  It must write and hash-seal all coupled predictions before
the scorer opens targets.

The primary teacher-capacity gate requires:

- `teacher_global` retains at least 70% of the ideal packet raw-MSE gain;
- each family retains at least 50%;
- at least three of the four rows improve in expected one-randomization MSE;
- `teacher_global` has lower target-free sampling MSE about the sealed packet
  reference than `independent`.

A failure kills this concrete response-signature/matching construction.  A
large failure by both teacher arms is strong evidence against local/groupwise
matching, but does not mathematically exclude an exact global output-space
coupling optimizer.

No FlopScope session, physical run, package, upload, submission, remote
action, or Mini100 access is authorized.
