# Walsh direction-state compression R2 — verdict

Date: 2026-08-06

## Verdict

**Hard kill.**  Restricting Walsh truncation to a short propagation window does
not rescue the mechanism.  Even the gentlest tested spelling (rank 192 only on
layers 12--23) worsened raw MSE by about `918x` on the two-row Full pilot and
`3583x` on the two-row Generated pilot.  This is far beyond any compute-saving
repayment threshold.

The screen was target-free through prediction sealing.  It used the exact K129
front, H2 repair, layer-4 snap, screens, late selection, and final readout; only
the specified within-basis 256-direction Walsh projection was changed.  Targets
were opened afterward solely to apply the preregistered gate.

## Results

Global-energy support, rank 128:

| projected layers | Full raw ratio | Generated raw ratio |
|---|---:|---:|
| 2--5 | 64,510.9 | 68,600.7 |
| 6--11 | 6,387.6 | 23,821.7 |
| 12--17 | 3,682.6 | 13,765.8 |
| 18--23 | 2,408.9 | 9,297.4 |
| 24--31 | 1,244.6 | 2,681.3 |
| 12--23 | 4,138.7 | 15,773.5 |

Higher-rank spellings on the actual expensive window, layers 12--23:

| retained rank | Full raw ratio | Generated raw ratio |
|---:|---:|---:|
| 160 | 2,141.7 | 8,442.4 |
| 192 | 918.5 | 3,582.6 |

## Structural conclusion

Average retained Walsh energy is not a useful fidelity measure across ReLU
layers.  Small discarded direction modes move samples across gate boundaries;
the resulting error survives and compounds even when projection is confined to
one window.  Do not reopen this lane by changing rank, support ordering, or
window endpoints.  Reopen only with an exact nonlinear closure for the omitted
modes, which would be a different estimator class.

## Evidence

- preregistration: `PREREG_WALSH_WINDOW_R2_20260806.md`
- sealed predictions: `runtime/artifacts/k129_walsh_window_r2_targetfree_20260806.npz`
- target-free receipt: `runtime/artifacts/k129_walsh_window_r2_targetfree_20260806.json`
- post-seal score: `runtime/artifacts/k129_walsh_window_r2_postseal_20260806.json`

Evidence label: **component** for the offline two-Full/two-Generated
falsification.  No FlopScope session, physical package run, upload, submission,
or remote action was performed.
