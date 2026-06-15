# Review: Forrelation - A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015)

Source note: [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]

## Primary Sources Checked

- Aaronson and Ambainis, "Forrelation: A problem that optimally separates quantum from classical computing", STOC 2015 / SICOMP 2018.

## Verdict

Major issues. Most of the Forrelation mechanics are good, but the assessment contains a false statement about Grover and total Boolean functions. This should be fixed before relying on the note.

## Line-Anchored Comments

- Add a top-level title.
- Lines 24-32: accurate summary of the main 1-vs-`tilde{Omega}(sqrt N)` separation and the simulation theorem.
- Line 34: false. Grover/search is a total Boolean function with quantum query complexity `Theta(sqrt N)`, not 1. The optimal constant-query versus `sqrt N` separation is for partial/promise functions. Replace the sentence with: "Earlier results gave exponential or large separations for promise problems, but Forrelation gives a natural problem attaining the optimal constant-query vs. `tilde{Omega}(sqrt N)` separation."
- Lines 40-52: the query-accounting caveat is good. The paper's one-query convention packages the pair of functions into a combined oracle; the circuit picture with one phase query to each function is clearer pedagogically.
- Lines 58-76: good summary of the Gaussian lower-bound strategy.
- Lines 112-128: clear statement of the optimality theorem.
- Line 149: good caveat about partial functions; this should be cross-referenced near line 34.
- Line 188: "earlier 1-vs-`Omega(2^{n/2})` exact quantum separation" is fine, but mention that the oracle conventions differ.

## Missing or Suggested Additions

- Add a short note on notation: `N=2^n` is the truth-table size, not the number of qubits in the query register. That prevents confusion when comparing to `n`-bit indexed functions.

