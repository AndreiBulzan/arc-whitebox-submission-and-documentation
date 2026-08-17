# Response-rank-8 conditional Gaussian mixture R2

Verdict: **kill**. Evidence label: **component**.

This is a current-challenge research result, but no FlopScope row was run.
The historical FlopScope 0.8.x scores are not used as an anchor. The only
deployment anchor used here is remote submission 320262 under FlopScope
0.9.1: raw `2.090467e-7`, effective compute `170.3B`, adjusted
`1.315388e-7`.

## Candidate

Let the mean-gated network Jacobian be `J`. Let
`V in R^(256 x 8)` contain its top eight input response directions. For
`x ~ N(0, I)`, condition on `xi = V^T x`:

```text
E[x | xi]   = V xi
Cov[x | xi] = I - V V^T.
```

For each cubature node, carry its conditional mean and full `256 x 256`
covariance through all 32 ReLU layers, using fourth-order Mehler Gaussian
covariance closure. Average the final conditional means. The tested positive
cross rule was

```text
xi in {+sqrt(8)e_i, -sqrt(8)e_i},  i=1,...,8;  weight = 1/16.
```

This is distinct from selecting existing cloud orientations by a Jacobian
score: the rank-8 gate geometry is explicitly integrated, while the other
248 dimensions are Rao-Blackwellized by full covariance.

## Fast falsifier

The capture opened weights only and sealed predictions before the scorer
opened `official_alm[-1]`.

```text
Full row 0
top-8 mean-gated response energy          0.9970681
ordinary full-covariance closure MSE      7.585122e-5
rank-8 conditional cross16 MSE            4.145289e-5
candidate / ordinary closure              0.546503
```

The response conditioning moves the analytic model strongly in the right
direction, but the absolute error remains about `198x` the remotely measured
K146 raw MSE. Even at the score multiplier floor of `0.1`, it would adjust to
about `4.15e-6`, over `41x` the `1e-7` goal.

This is closure bias, not merely the fourth-order Mehler truncation. The
existing broad exact-bivariate full-covariance Gaussian screen is also at
roughly `6.7e-5` Full MSE. More rank-8 nodes cannot repair the repeated
Gaussian projection at every hidden layer.

## FlopScope 0.9.1 economics (projection, not receipt)

One full-covariance component needs two dense `256 x 256` matrix products per
layer:

```text
32 * 2 * 256^2 * (2*256 - 1) = 2.143B arithmetic FLOPs/node
cross16 covariance products                  about 34.3B
one baseline closure + response basis         about  2.2B
elementwise closure and runtime residual       unmeasured
```

A participant-safe response basis would avoid a `256 x 256` SVD: propagate
8--16 fixed Walsh probes backward through the gated linearization and
orthogonalize the resulting `256 x r` block. That basis construction is well
below `0.2B` projected arithmetic FLOPs. The core graph is therefore
economically interesting, likely around `37--40B` before current-meter
residual and special-function tariffs, but accuracy kills it before a
physical 0.9.1 implementation is warranted.

## Sealed evidence

- target-free capture:
  `cross16_full0_targetfree_r2.npz`,
  SHA-256
  `0a766fb123f3f7b562722795b20cf634eeee9963242bc4443a0aeba962827d22`
- capture manifest: `cross16_full0_targetfree_r2.json`
- post-seal score: `cross16_full0_postseal_r2.json`
- capture source:
  `capture_response_rank8_conditional_mixture.py`
- scorer source: `score_cross16_full0_postseal_r2.py`

No physical rows, FlopScope sessions, remote actions, or submissions occurred.
The scout acquired the shared benchmark lock only to avoid perturbing another
lane's wall measurement.
