# K162 conditional-width R1 — decisive kill

Date: 2026-07-29

Evidence: **component** target-free CUDA predictions followed by a sealed
score on already-used Full4/Generated4 rows. All current-meter and adjusted
numbers are **projection**. No physical FlopScope row, package, network
action, upload, or remote submission occurred.

## Result

The candidate reaches the requested compute territory, but it does not
preserve accuracy:

```text
R19 steady effective                         181.151096762 B
conditional s8/w200 projected effective      162.304174506 B
projected effective reduction                    10.4040%

Full pooled raw-MSE ratio                           1.66793
Generated noise-corrected pooled ratio              2.05793
Full / Generated maximum row ratio           2.3611 / 2.3823
Full / Generated rows improved                    1 / 0 of 4
```

Every preregistered accuracy gate failed. Applying those component ratios to
the current R19 remote-calibrated projection gives:

```text
Full projection                         1.763e-7 .. 1.823e-7
Generated projection                    2.176e-7 .. 2.249e-7
```

This is not a `1.2e-7` candidate.

## What was tested

The unchanged K162/m33 control keeps width 232 through layer 11 and width
216 through layer 20, transmitting omitted coordinates by their analytic
mean. The single fixed candidate keeps width 200 from layers 8 through 20.
At every checkpoint it reconstructs omitted fluctuations with the
target-free minimum-MSE Gaussian linear conditional:

```text
B = pinv(C_KK) C_KD
h_D ~= mu_D + (h_K - mu_K) B
```

`B W_D` and the corresponding centered mean fold are composed into the next
right, so no omitted trajectory array is materialized. The inverse cutoff,
start layer, width, energy ranking, support, and rows were all fixed before
prediction. The control associated with the frozen physical K162 Full0
prediction at:

```text
relative RMSE      1.16658e-6
maximum absolute  9.80662e-6
```

The target-free archive was sealed before either target dataset was opened.

## Prior depth/compression audit

This test was selected only after checking the existing negative boundary:

- checkpoint direction thinning saved `13.5--27.1%` counted work but
  worsened raw MSE by `1.48--4.91x`;
- late hidden-width compression saved only `3--6B` and worsened raw by
  `1.79--42x`;
- sensitivity-ranked early width reduction reached the compute band but
  worsened raw by `4.29/4.57x` at s7/w192;
- mean/covariance-preserving checkpoint rejuvenation reached `135.3B`
  projected effective but worsened raw by `4.61/6.71x`;
- endpoint multilevel width correction needed too much high-fidelity support
  and never beat the adjusted incumbent;
- delayed basis pruning and L8 checkpoint-response multifidelity already
  failed their two-family gates.

Analytic conditional reconstruction was the one explicitly untested
successor left by the sensitivity-fold verdict. R1 now closes that successor
at the first schedule large enough to save at least 10% effective.

## Mechanistic conclusion

The retained analytic post-ReLU covariance does not carry enough of the
realization-specific omitted fluctuation. The fitted conditional maps were
numerically well-defined, but their Frobenius norms were generally small and
they recovered far too little of the directed, higher-than-quadratic
trajectory state. This is consistent with the independent checkpoint
rejuvenation result: preserving or reconstructing means and covariances does
not preserve this estimator's useful sign-bearing connected chaos.

Do not scan nearby widths, start layers, inverse cutoffs, ridge values, or
energy/sensitivity rankings. Reopen depth-selective width compression only
with a new higher-order observable that directly transports the omitted
directed state, not another Gaussian conditional or covariance factor.

## Sealed artifacts

```text
82cea26019f691330692a7b7083b5cbf4c37f078b17d31e63cd0cb16863eaf2d
  PREREG_K162_CONDITIONAL_WIDTH_R1_20260729.md

4e9f646d309ef27abcdabe75e44d61709ef5475d817dced480a97c29cae8ce38
  capture_k162_conditional_width_r1_targetfree_20260729.py

85204f4b7589c922b8a8a407c9fb6e876faab31bc957be5a1e0ec64c6e302365
  k162_conditional_width_r1_targetfree_20260729.npz

f9cc31ed26ffd3bd2185449a67212516891971a28a063adbb5b1a14cb3d8dbbc
  k162_conditional_width_r1_targetfree_20260729.json

98743471196aeb044688e5c7e80195f50f3058137f916e6acbf6e7fe9cca28e2
  score_k162_conditional_width_r1_postseal_20260729.py

6209eeeb5cf305264ca9813ec0025856d4a90b4fe2f7806af9be859478ade10e
  k162_conditional_width_r1_postseal_score_20260729.json
```
