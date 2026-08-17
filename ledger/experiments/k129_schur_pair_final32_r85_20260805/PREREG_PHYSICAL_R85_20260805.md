# K129 R85 physical final-path32 implementation

Date: 2026-08-05

Implement only the broadly confirmed `final_path32` island over exact R84.
The polar constructor remains R83 float64 polar-eigh.  The relative matrix,
final symmetric eigensolver, downstream-sensitivity selector, and hybrid
reconstruction use float32; propagated K129 logic and the R84 output scale are
unchanged.

The exact archive gate is decisive.  Require bare setup, initialized and
steady predictions, persistent identity, counted/effective budget compliance,
and association to the sealed final-path32 Full row 17.  Promote only if the
steady counted ledger is strictly below R84's 126,297,542,990 FLOPs and the
association is within 1.25x the existing R84 statistical/physical parent
error plus the measured final-path32/control displacement.

