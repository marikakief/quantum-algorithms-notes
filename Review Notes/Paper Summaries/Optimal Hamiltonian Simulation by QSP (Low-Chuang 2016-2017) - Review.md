# Review: Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017)

Source note: [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]

## Primary Sources Checked

- Low and Chuang, "Optimal Hamiltonian Simulation by Quantum Signal Processing", PRL 2017 / arXiv:1606.02685.

## Verdict

Minor omissions. The note has the right conceptual message and the main complexity statement is essentially right. It is short enough that it omits several theorem-level constraints that matter for a review vault.

## Line-Anchored Comments

- Add a top-level title; the file currently begins with metadata after a blank line.
- Lines 10-21: good conceptual framing of signal transduction, transformation, and projection.
- Lines 27-31: the Jacobi-Anger/Bessel approximation is the right approximation engine to highlight.
- Lines 35-39: the complexity statement should define the simulation parameter, e.g. `tau = t d ||H||_max` or the appropriate normalized block-encoding parameter. Without that, "matches lower bounds in all parameters" is too compressed.
- Lines 43-56: good historical positioning. The companion qubitization paper should be described as more than "cleaner notation": it generalizes the signal model and standard-form encoding framework.
- Lines 58-65: the "References within this paper" section includes Haah 2019, which cannot be a reference within the 2017 PRL. Move later papers to cross-links.
- Lines 77-80: these later cross-links are useful, but they underscore that the source note itself should separate the PRL result from later developments.

## Missing or Suggested Additions

- State the QSP polynomial admissibility conditions: parity, boundedness, and relation between polynomial degree and query count.
- Add one sentence on why the `log(1/epsilon)/loglog(1/epsilon)` precision term is optimal by known lower bounds.
