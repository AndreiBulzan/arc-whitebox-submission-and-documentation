# K26 low-compute observer audit R1

Date: 2026-07-29

Evidence label: **component diagnostics**.

## Decision

**No K26 successor is promoted.**

The exact historical K26 chassis remains an important low-compute reference,
but none of the tested compact observers supplies the independent error signal
needed to move it to the `<=1.15e-6` both-family raw gate.  No physical row,
package, upload, submission, or remote action was performed.

## Controlling K26 anchors

The exact historical archive is AIcrowd submission `315968`:

```text
remote raw final-layer MSE        1.299467404579e-6
remote adjusted score            1.427328043128e-7
historical FlopScope             0.8
```

On the exact D56 Generated128 replay under one-million-sample labels:

```text
observed raw MSE                  1.755374318863e-6
mean label-noise MSE              4.983059848612e-8
noise-corrected raw MSE           1.705543720377e-6
```

On the independent synthetic1024 K26 trajectory bank:

```text
all-row raw MSE                   1.6773865e-6
strict test subset raw MSE        1.613178800285e-6
```

Therefore the public remote aggregate alone is not enough to claim a
`<=1.15e-6` transferable K26 candidate.

## 1. Within-MLP empirical-Bayes smoother

The preregistered target-free smoother used the 256 exchangeable output
coordinates as its internal regression population.  It fitted every output
out-of-coordinate-fold and selected only on Full indices `600..799`.

The least-bad nonzero candidate was already a decisive failure:

```text
candidate                         linear, ridge 1, blend 0.10
Full fit ratio                    4.423767174
Full validation ratio             4.430129423
sealed Full test target opened    false
```

The test target was deliberately not opened because the continuation gate
failed before test.

## 2. Full-covariance Gaussian absolute readout

The existing screened full-covariance rollout already carries a covariance
matrix through all hidden layers but discards its final projected variance.
As a diagnostic, the final variance was projected through `W31` and combined
with the corresponding Gaussian ReLU expectation.

This absolute readout was badly biased on Generated128:

```text
K26 observed raw                  1.755374319e-6
Gaussian absolute raw             7.562441401e-5
correction/residual correlation   0.07038495
```

A scalar blend fitted on Generated rows `0..63` transferred to rows `96..127`
at only:

```text
base test raw                     2.070153382e-6
blended test raw                  2.055161835e-6
ratio                             0.99275824
```

This is below one percent and nowhere near the required gain.  It also costs
an additional final covariance projection, so it is closed.

## 3. Frozen gauge-innovation cell driven by paid K26 state

The strongest new transplant tested the exact frozen R1 step-20
gauge-equivariant innovation cell without retraining.  Instead of autonomous
rollout, every layer was teacher-forced by the already-paid K26 post mean and
second moment from:

```text
cache/root_k26_l4_trajectory_synthetic1024_20260715.npz
```

This is the most favorable lawful deployment interface for that checkpoint:
it avoids free-rollout drift while preserving its recurrent signed state.
Rows `0..7`, touched by an exploratory smoke, were excluded.  The remaining
rows used the historical modulo-eight train/validation/test split.

```text
                         K26 base raw     frozen-cell raw      ratio
train, 762 MLPs          1.697320711e-6   3.408138287e-6       2.007952
validation, 127 MLPs     1.658285652e-6   3.448495199e-6       2.079554
test, 127 MLPs           1.613178800e-6   3.342562445e-6       2.072035
```

The frozen correction had essentially zero alignment with the K26 residual:

```text
train correlation                 0.0003439
validation correlation            0.0076420
test correlation                  0.0044727
train-optimal blend lambda         0.00024964
test ratio at that lambda          0.99999555
```

The useful Stage-A teacher signal does not survive conversion into a K26
residual observer.  Do not retrain or deploy this transplant.

## 4. Global final-seed reweighting

The synthetic1024 bank also permits an inexpensive same-compute check of the
26 final seed coordinates.  A sum-preserving linear reweighting was fitted on
762 MLPs, selected on 127, and evaluated on the remaining 127.  The
truth-trained ridge winner changed the strict test MSE only from

```text
K26 base                         1.613178800e-6
reweighted final seed mean       1.610932578e-6
ratio                            0.99860758
```

A deterministic K258-teacher fit reversed on the strict test (`1.00257x`).
This independently confirms the older Full OOF seed-weight failures: the
remaining error is not recoverable by another global weighting of the same
26 final seed responses.

## 5. Final-column zonoid smoothing

No duplicate K26 run was launched.  This exact mechanism has now been tested
more directly twice:

1. `final_weight_spectral_shrink_20260729` used the exact centered covariance
   kernel of absolute Gaussian projections and the degree-two even kernel.
   The selected interpolation strength collapsed to `0.00718` and reversed on
   both held families (`1.000176x` Full, `1.000122x` corrected Generated).
2. The earlier H30-covariance-metric zonoid query smoother tested 780
   rank/kernel/protection/ridge configurations.  Bidirectional Full selection
   chose the literal no-op; even its truth-open best-line capacity captured
   only about `0.7--0.8%`.

Those experiments test the relevant cross-output information source more
strongly than repeating the same W31 kernel on a noisier K26 endpoint.

## Consequence

The historical K26 route is not discarded, but it has no banked compact
accuracy correction.  A viable floor candidate now requires a genuinely
independent observer or a new quadrature construction; smoothing, marginal
Gaussian completion, the frozen gauge cell, and W31 output-direction kernels
do not provide it.
