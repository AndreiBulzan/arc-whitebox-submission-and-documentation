# Preregistration: covariance-matched packet coupling R2

Date frozen: 2026-08-08

R1 matched response functions in an ordinary Euclidean metric and failed.
That metric is wrong once the response contains an even-in-noise component.
Writing the centred response as `E_i(z)+O_i(z)`, with `E_i(-z)=E_i(z)` and
`O_i(-z)=-O_i(z)`, a cross-centre antithetic pair has covariance term

```text
<E_i,E_j> - <O_i,O_j>.
```

Thus odd responses should align, while even responses should anti-align.
Euclidean response matching aligns both and can increase variance, exactly as
R1 did.

R2 freezes the corrected pair cost

```text
cost(i,j) = <E_i,E_j> - abs(<O_i,O_j>),
```

where the absolute value optimizes the free orientation of line `j`.
Eight common Gaussian directions and a fixed 32-column output projection
estimate the two inner products.  Training and evaluation noises are
disjoint.

The rows, epsilon, normalization, eight evaluation repetitions, sealing
rules, and primary gates are unchanged from R1.  The frozen arms are
`independent`, `random_pair`, `natural_basis`, `cov_teacher_basis`, and
`cov_teacher_global`.  The global teacher clusters canonicalized odd-response
features into 512 clusters, applies the covariance cost inside each cluster,
then pairs leftovers by the same cost.

Pass requires the global teacher to retain at least 70% of ideal packet gain,
each family at least 50%, at least three of four rows to improve, and lower
target-free sampling MSE than independence.

No physical run, FlopScope session, Mini100 access, package, upload,
submission, or remote action is authorized.
