# Deep spherical-Stein control R1 verdict

Date: 2026-07-29

Evidence label: **component**.  This was a 2 Full + 2 Generated fast screen,
not broad statistical evidence.  It used no FlopScope session, physical row,
package, upload, submission, or remote action.

## Verdict

**Kill R1.**  The exact identity is correct, but four actual deep-gradient
controls do not predict the structured quadrature error.

The independent IID check on 32,768 spherical points passed:

```text
median |z(mean control)|   0.211
p95    |z(mean control)|   1.719
max    |z(mean control)|   2.693
```

The stable point-cross-fitted spelling changed raw MSE by only a few percent:

```text
                       Full ratio   Generated corrected ratio
K16, four controls        0.9836              0.9763
K32, four controls        1.0049              0.9654
required                 <=0.5000             <=0.5000
```

Regressing complete-basis means was unstable and reversed by family.  Its
most tempting member, K32/four controls, reached `0.5440` on Full but
`1.5538` on Generated.

Thus the deep Stein field carries ordinary pointwise variance signal but
essentially none of the deterministic complete-basis integration error.
Adding directions would multiply propagation cost without a plausible route
to the required gain.  Reopening this family would require a different
nonlocal vector field with a target-free coefficient, not more fixed input
directions or more ridge tuning.

Authoritative artifacts:

- `deep_stein_control_r1_targetfree_20260729.npz`
  (`3035d225c76994661ef2a2ffa2de5d7ef3e75354ca9a62eb454a1d8418c4013e`)
- `deep_stein_control_r1_postseal_20260729.json`
