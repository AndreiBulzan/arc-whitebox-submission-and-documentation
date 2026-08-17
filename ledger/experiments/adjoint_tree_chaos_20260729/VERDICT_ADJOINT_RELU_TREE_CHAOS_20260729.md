# Adjoint ReLU tree-chaos final-transition gate

Date: 2026-07-29

Evidence label: **component**. This is a two-phase, teacher-forced
final-transition study on 32 already-open Full development MLPs. It is not a
free rollout, measured whole, broad statistical result, physical price,
package, upload, submission, or remote result.

## Verdict

**Kill the parameter-free Gaussian/ReLU leading-tree closure as a route to
the present `1.2e-7` checkpoint. Do not build a recurrence or physical
candidate from it.**

The strongest tested arm uses exact score-time layer-31 K3 and exact
teacher layer-30 preactivation mean/covariance. Even with those oracle-state
advantages, it misses the hard gate:

| final-transition arm | pooled final-mean MSE |
|---|---:|
| exact K3, no K4 | `1.673833e-7` |
| exact K3 + H1/H2 path tree K4 | `1.471639e-7` |
| exact K3 + all leading H1/H2/H3 tree K4 | `1.610260e-7` |
| exact K3 + leading **all-distinct-only** tree K4 | **`1.332059e-7`** |
| exact K3 + exact pair K4 + leading all-distinct trees | `1.437131e-5` |
| exact K3 + exact K4 oracle | `1.977727e-8` |

The best arm improves the exact-K3 endpoint by `20.42%`, so the diagram is
not numerically inert. It remains `11.0%` above the preregistered
`1.2e-7` capacity threshold before replacing either exact teacher
pre-covariance or exact score-time K3 by a production estimate.

More decisively, the best all-distinct K4 query has correlation `-0.0473`
and relative RMSE `1.00286` against the true K4 query. Its readout gain is a
small aggregate sensitivity/bias effect, not recovery of the missing
weight-specific tensor. It is not a robust basis for a 32-layer chain.

## Parameter-free derivation tested

For standardized jointly Gaussian `Z` with correlation `R`, write the
centered coordinatewise ReLU in probabilists' Hermites:

```text
ReLU(mu_i + sigma_i Z_i) - E[.]
  = a_i H1(Z_i) + c_i H2(Z_i) + g_i H3(Z_i) + ...

a_i = sigma_i Phi(mu_i / sigma_i)
c_i = sigma_i phi(mu_i / sigma_i) / 2
g_i = -sigma_i (mu_i / sigma_i) phi(mu_i / sigma_i) / 6
```

For one realised downstream query `w`, define

```text
b = (a .* w)
d = (c .* w)
e = (g .* w)
u = R b
```

The leading connected all-coordinate contractions are then query-direct:

```text
K3 tree =  6 sum_i d_i u_i^2

K4 path = 48 (d .* u)^T R (d .* u)
K4 star = 24 sum_i e_i u_i^3
```

These are the complete weak-correlation tree topologies at these orders:
the K3 `H2-H1-H1` path and the K4 `H2-H2-H1-H1` path plus
`H3-H1-H1-H1` star. They contain three- and four-distinct-index terms
without materializing K3/K4 tensors.

R1 also derived the implied repeated views analytically and subtracted their
literal pair contraction, leaving only the all-distinct tree term. For
example:

```text
C21_tree(i,i,j)
 = 4 c_i a_i a_j R_ij + 2 c_j a_i^2 R_ij^2
```

The corresponding closed-form `C22/C31` formulas were checked
algebraically: for dimensions one and two, the full query equals the pair
query to floating-point precision, so the purported all-distinct residual
is zero exactly where it must be.

## Why it fails

The leading weak-correlation diagrams are far too small and have the wrong
query geometry at depth 31:

| query | correlation | relative RMSE |
|---|---:|---:|
| exact repeated/pair K4 | `0.41585` | `0.88378` |
| R1 pair + H1/H2 all-distinct tree | `0.42168` | `0.88126` |
| R2 full leading-tree K4 | `-0.06802` | `1.00508` |
| R2 all-distinct-only leading-tree K4 | `-0.04730` | `1.00286` |

The exact pair contribution is itself dangerous because the true
all-distinct tensor cancels it. The Gaussian tree expansion does not
recover that cancellation. Adding exact repeated-index corrections
therefore produces `~1.4e-5`, essentially the already-killed literal-pair
failure.

This directly distinguishes the result from PTCC. PTCC reconstructs one
globally consistent MLP-specific core from exact teacher repeated moments
and reaches K4 correlation `0.9943`; the present closure tries to predict
the missing core from Gaussian pre-mean/covariance alone and cannot.

## Compute boundary

The final-transition query itself is cheap in ordinary algebra:

```text
R @ (a .* W)
R @ (d .* (R @ (a .* W)))
```

plus elementwise reductions, about two dense width-256 matrix products, or
roughly `0.067B` multiply/add operations for all 256 outputs. This is a
**projection**, not a FlopScope receipt.

Cheapness does not rescue the state. The best capacity result already uses
exact teacher covariance and exact downstream K3, yet fails the hard gate.
ARC's public implementation also confirms why carrying the omitted higher
orders literally is not the answer: factor ranks grow by width per layer.
Higher-loop enumeration would reopen the full tensor/state problem without
evidence that the first omitted loop closes the measured gap.

## What survives

Only a narrow observation survives: leading all-distinct ReLU diagrams can
move the exact-K3 readout in the useful direction at negligible
final-query cost. Reopen this class only if a future estimator exposes a
new MLP-specific connected state that fixes the query correlation—such as a
repaired PTCC-like core upstream. Do not reopen by adding more
mean/covariance-only Gaussian loops, fitting a scalar coefficient, or
rolling the same local correction through depth. The public discussion's
reported error compensation makes the latter especially unsafe.

## Reproducible artifacts

```text
R1 H1/H2 source
  run_relu_q2_tree_final_transition_20260729.py
  d6c2e1e001e56ed9ee4ff75766057dd59d42cc377a5ed67d21c42a30c6ff0f09

R1 target-free capture
  relu_q2_tree_full32_l31_capture_r1_20260729.npz
  b9610f7f70cb4ec076ecd4bb6f83bb8c6cbafe79c511e98c7347306da4b61938

R1 score
  relu_q2_tree_full32_l31_score_r1_20260729.json
  6b28bbb1ed178e44b558a5a798ee28f259c6fb3025a90fad2ffe6f9341851f5a

R2 H3-star source
  run_relu_q23_tree_final_transition_r2_20260729.py
  1b8bd26aabbdc7e1518e1aa250c78e1a6db517e69a64e75d26ae3e308b3ec668

R2 target-free capture
  relu_q23_tree_full32_l31_capture_r2_20260729.npz
  00724b2c1b3c0087faf54561c3f63f3a311afe4ecf69fe1d31e4bc7d0870a768

R2 score
  relu_q23_tree_full32_l31_score_r2_20260729.json
  04c7a94771ddab3bc1d7123574c39f0cf185f2c9c0829d0dec388ac28446025b
```

No frozen deployment source, `STATUS.json`, physical benchmark lane,
package, or remote state was changed.
