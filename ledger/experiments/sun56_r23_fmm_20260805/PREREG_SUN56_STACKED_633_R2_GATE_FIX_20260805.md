# Sun-56 stacked 6x3x3 R2 — promotion-gate correction

R1 verified the identity and completed the census, but its final Boolean
mistakenly promoted any saving already present on the pre-existing global
frontier.  Its own report shows `sun_changed_groups = 0` at every allowance,
so that label is false and must not be used.

R2 changes no scheme, census, option, or price.  It corrects the gate to
require at least one selected `Sun56Stack633` group in addition to the frozen
saving threshold, and writes a new receipt rather than overwriting R1.
