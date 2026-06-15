# Review: Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferova-Scherer-Berry 2018)

Source note: [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]

## Primary Sources Checked

- Kieferova, Scherer, and Berry, "Simulating the dynamics of time-dependent Hamiltonians with a truncated Dyson series", PRA 2019 / arXiv:1805.00582.

## Verdict

Minor issues. The note gives a good account of the ordered-time LCU problem and the two tuple-preparation constructions. The main corrections are parameter precision and a few overcompressed complexity statements.

## Line-Anchored Comments

- Add a top-level title.
- Lines 10-22: good problem statement and good identification of time ordering as the obstacle.
- Lines 38-49: the Dyson series segment expression is clear.
- Lines 53-58: the sparse decomposition parameters need more care. `gamma`, `L`, and `H_max` are not just bookkeeping; they affect query/gate counts and should be defined using the paper's notation.
- Lines 72-82: excellent summary of gap encoding versus sorting.
- Line 90: "single-step OAA brings success probability to 1" should be softened to "implements the segment coherently up to the target approximation error." Robust OAA is not magic exactness when the truncated/discretized segment is only approximately unitary.
- Line 92: `gamma = Theta(epsilon/(d^3 t))` is too specific and surprising as written. Recheck this against the paper's sparse-decomposition and discretization parameters; it may be a gate-precision choice rather than the main simulation scale.
- Lines 100-109: the headline query expression should keep `d^2 H_max t` consistently inside the logarithm if that is the chosen effective `lambda t`.
- Lines 155 and 140: use `log/loglog` or "polylogarithmic" rather than plain `O(log 1/epsilon)` when comparing to truncated Taylor.
- Lines 172-180: split actual references from later cross-links; Low-Wiebe and later papers are not references inside the 2018 arXiv paper if they postdate it.

## Missing or Suggested Additions

- Add the dependence on derivative/time-discretization oracles explicitly. The note says derivative dependence enters through discretization, but a theorem-level summary should say which smoothness parameter controls `M`.
