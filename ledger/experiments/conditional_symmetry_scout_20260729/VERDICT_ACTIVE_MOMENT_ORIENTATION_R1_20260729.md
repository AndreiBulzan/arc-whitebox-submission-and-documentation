# Active-moment orientation R1 verdict

Date: 2026-07-29

## Verdict

**Hard kill.  Do not broaden or build a runtime successor.**

This was a target-free **component** capture followed by a separately opened
signed-final-preactivation score on eight Full and eight Generated rows.  No
GPU, FlopScope session, physical row, package, upload, submission, or remote
action occurred.

The mechanism was lawful and genuinely weight-conditioned: it chose the
literal `129:17` ordered Kerdock orientation pair that best matched the exact
uniform-sphere fourth moments inside the weights-only rank-eight active input
subspace.  That subspace was strongly concentrated:

```text
mean top-8 mean-gated Jacobian energy
Full       0.98267
Generated  0.98460
```

The concentration did not translate into quadrature-error information.

| family | fixed signed MSE | candidate signed MSE | ratio | row p95 | rows improved |
|---|---:|---:|---:|---:|---:|
| Full8 | `4.97377e-7` | `8.72073e-7` | `1.75335x` | `3.43159x` | `25%` |
| Generated8 | `7.30292e-7` | `5.71507e-7` | `0.78257x` | `1.50813x` | `50%` |

The preregistered gate required `<=0.50x` pooled error, p95 `<=1.50x`, and
at least `75%` of rows improved in both families.  It failed every Full gate
and all three Generated gates.  The reciprocal behavior is decisive enough
that a broader capture would only spend selection budget.

## Learned boundary

A deep random ReLU MLP can have an extremely concentrated local/mean-gated
input Jacobian while its *integration error* remains governed by gate-cell
geometry outside that tangent subspace.  Matching fourth moments after a
weight-conditioned orthogonal projection is therefore not a usable
Rao--Blackwell signal here.  This closes the active-subspace
fourth-moment/orientation spelling, in addition to the already-killed
first-layer QR and fixed great-circle rules.

Reopen only with a nonlocal observable that measures actual late gate-cell
innovation, not another local Jacobian rank, projected spherical moment, or
orientation-pair rule.

## Seals

```text
preregistration
586af0439f45d729fb203fb95bd217f67e95c8fabac3cbe7b36a569e211d1ccb

capture source
967141293e7387a815244c63f7383cd915daecc9c8e80ed75e89b5b1bacf851d

target-free arrays
aabbd955b0c38fbca192f2938e45899c0a3d8a3ee49a19c6bffc419e16d4e405

post-seal scorer
6b9df97fe24fcd8b91d22bc7c1ed31279dff0f26b60020a8adda1dd630f42d39

post-seal score
3da886272801341b2c20bfac5a0c30294f2624ce8d44955005149cd91bb02740
```
