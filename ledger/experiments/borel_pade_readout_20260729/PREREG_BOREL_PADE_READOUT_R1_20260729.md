# Final-preactivation Borel–Padé readout R1

Date: 2026-07-29

Evidence boundary: this is a target-free, offline **component** falsifier.
It is not a FlopScope row, measured whole, package, upload, submission, or
remote result.

## Frozen question

Can a standard Borel–Padé resummation of the final-preactivation marginal
Edgeworth series replace the current K146/m17 readout and reduce pooled raw
MSE by at least 12% on both the Full and Generated process families?

The only acquisition rows are:

```text
Full       0, 1, 2, 3
Generated  2, 4, 5, 6
```

These are already-open development rows in the frozen K146 broad bank. No
Full confirmation row at or above 200 may be opened.

## Frozen moment convention

For each K146 arm independently, let `z` be its existing unit-sphere final
preactivation cloud. For `r=1..8`, estimate the radial raw moment

```text
m_r = rho(256, r) * mean(z**r)
```

over every sign, basis, and within-basis direction with literal equal
weights. Convert raw moments to cumulants with

```text
kappa_n = m_n - sum(
    choose(n-1, j-1) * kappa_j * m_(n-j), j=1..n-1
)
```

where `m_0=1`. Set `sigma^2=kappa_2`, `lambda_r=kappa_r/sigma^r`, and
`a=-m_1/sigma`.

Introduce the standard Edgeworth bookkeeping parameter `t`, under which
`lambda_r` has order `t**(r-2)`:

```text
E(t,D) = exp(sum(lambda_r * t**(r-2) * D**r / r!, r=3..8))
```

Expand `E(t,D)` through `t**6`. If the coefficient of `t**q D**n` is
`e[q,n]`, the ReLU series coefficient is

```text
c_0 = m_1 * Phi(m_1/sigma) + sigma * phi(a)
c_q = sigma * phi(a) * sum(e[q,n] * He_(n-2)(a), n>=2)
```

using probabilists' Hermite polynomials. This identity follows from
integrating `ReLU(m_1 + sigma*x) phi(x) He_n(x)`.

## Frozen Borel–Padé definitions

The Borel coefficients are `b_q = c_q / q!`.

For `[L/2]`, with `L` equal to 2 or 3, choose

```text
Q(u) = 1 + q1*u + q2*u**2
P(u) = p0 + ... + pL*u**L
```

so that `Q(u) * sum(b_q*u**q) - P(u)` vanishes through order
`u**(L+2)`. The two denominator equations are solved directly without
ridge regularization or fitted constants. A rank-deficient denominator
system is a target-free failure for that order.

The Borel sum at `t=1` is fixed as

```text
integral_0^infinity exp(-u) P(u)/Q(u) du
```

evaluated by the following fixed 12-point Gauss–Laguerre rule:

```text
nodes =
0.1157221173580205, 0.61175748451513079,
1.512610269776419, 2.8337513377435068,
4.5992276394183484, 6.8445254531151773,
9.6213168424568671, 13.006054993306348,
17.116855187462257, 22.151090379397004,
28.487967250983999, 37.099121044466919

weights =
0.26473137105543654, 0.37775927587314212,
0.24408201131987906, 0.090449222211681821,
0.020102381154634218, 0.0026639735418653335,
0.00020323159266300131, 8.3650558568199259e-06,
1.6684938765409212e-07, 1.3423910305150041e-09,
3.0616016350351023e-12, 8.1480774674260935e-16
```

Each arm result is clipped only to the target-free moment bounds
`max(0,m_1) <= E[ReLU(Z)] <= sqrt(m_2)`, then the two arms are combined with
the production literal weights `129/146` and `17/146`. There is no fitted
coefficient and no target-dependent order choice.

## Target-free stability gates

An order is killed before target access if any arm/output coordinate has:

1. a singular Padé denominator system;
2. a real pole of `Q` anywhere on the positive integration axis;
3. absolute denominator below `1e-8` at any fixed quadrature node;
4. a nonfinite coefficient or candidate.

The whole lane is killed without opening targets if both orders fail. If at
least one survives, freeze and hash the target-free capture before scoring.

## Post-seal score gate

For each surviving order, compare its complete blended prediction to the
captured current K146 prediction on the same rows. Generated loss is
noise-corrected using the existing label-noise array.

Kill unless one single predeclared order satisfies all of:

```text
Full pooled raw-MSE ratio vs current       <= 0.88
Generated pooled corrected ratio           <= 0.88
Full row-ratio p95                          <= 1.25
Generated row-ratio p95                     <= 1.25
```

Any family reversal or either pooled ratio above `0.88` is an immediate
kill. Passing eight rows would license a separately sealed broad screen; it
would not promote an estimator.

## Count ceiling

Seven recursive powers and eight reductions over the existing 74,752 by
256 final-preactivation elements cost under `0.30B` primitive arithmetic
operations. Padé construction and quadrature are width-only. The hard
incremental projection ceiling is `2B`, including conservative request and
storage allowance. No physical receipt is authorized by this protocol.

