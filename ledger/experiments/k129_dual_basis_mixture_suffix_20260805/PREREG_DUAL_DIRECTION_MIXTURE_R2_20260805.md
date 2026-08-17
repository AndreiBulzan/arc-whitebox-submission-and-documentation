# Dual-frame direction-mixture R2

Date: 2026-08-05

Evidence sought: target-free **component** capture followed by a fixed
Full4/Generated4 falsifier. Any compute/adjusted value is a **projection**.
No package, upload, submission, or remote action is authorized.

R1 showed that whole-basis selection is checkpoint invariant and reverses
between Full and Generated. R2 mixes at the finer antipodal-direction level
inside every basis. It is distinct from the prior endpoint coresets, which
select and weight whole basis endpoints, and from sparse secondary arms,
which add separately repaired basis trajectories.

For each of 129 bases and each of 256 direction labels, both antipodes come
from one arm. Three masks are fixed before capture:

1. `direction_parity`: q0 for even direction labels;
2. `checkerboard`: q0 when `(basis + direction)` is even;
3. `walsh_character`: q0 when the parity of
   `direction & (basis + 1)` is even.

Each basis receives exactly 128 q0 and 128 polar antipodal directions. The
hybrid is formed immediately before the existing layer-4 per-basis snap, so
the deployable graph is one ordinary K129 trajectory, not a two-prefix graph.
The polar frame-construction component costs `1.748667394B`; candidate
economics use a conservative `142.5B` effective projection.

Pilot rows are Full `{7,17,27,37}` and Generated `{0,1,2,3}`. Select the mask
minimizing the worse projected adjusted score after sealing. Promote one mask
to official Mini100 only if both families project to at most `1.10e-7`, have
raw ratio at most `0.94`, and improve at least 3/4 rows.

