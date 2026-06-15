# Review: Quantum Complexity Theory (Bernstein-Vazirani 1993)

Source note: [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]

## Primary Sources Checked

- Bernstein and Vazirani, "Quantum Complexity Theory", STOC 1993 / SIAM J. Comput. 26, 1411-1473 (1997), DOI: https://doi.org/10.1137/S0097539796300921
- Full-text PDF checked: https://web.cs.miami.edu/home/burt/learning/csc595.251/Bernstein-Vazirani-QTM.pdf
- Shor's historical discussion of BV checked: https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Major issues. The note correctly identifies the paper's importance, but it conflates the simple Bernstein-Vazirani hidden-string algorithm with the paper's stronger recursive/oracle separation.

## Line-Anchored Comments

- Line 13: "first superpolynomial ... separation ... One query suffices ... vs. n"

  This conflates two results. The basic hidden linear function problem is a 1 versus n query separation, i.e. polynomial. The superpolynomial bounded-error oracle separation comes from the recursive Fourier-sampling construction in the paper. Source check: the SIAM version says the oracle problem is polynomial-time quantum but not solvable in `n^{o(log n)}` probabilistic time; Simon later strengthens this to exponential.

- Lines 45-48: "Classically, this requires Omega(n); quantumly ... O(log n)"

  This is not the right asymptotic for the headline recursive separation as stated in the paper. It makes the recursive result look merely polynomial. Rewrite this section around recursive Fourier sampling and the `n^{Omega(log n)}`-type classical lower bound / `poly(n)` quantum upper bound, using the paper's parameterization.

- Line 11: "BQP subset PSPACE"

  The source abstract and main text emphasize `BPP subset BQP subset P^#P` for the SIAM paper. `BQP subset PSPACE` is true by standard path-summing simulation and follows from `P^#P subset PSPACE`, but it is not the sharp containment highlighted here. Prefer the paper's stated containment, then mention PSPACE as a weaker corollary.

- Line 57: "expected time T ... exact time O(T)"

  Needs a source-precise formulation. BV deal with well-formed QTMs, halting, precision, and efficient universal simulation; "expected to exact in O(T)" is too casual and may not match the theorem statement.

- Lines 66-68: historical lineage

  Good, but distinguish "BV as hidden linear problem" from "BV as recursive Fourier-sampling oracle separation." Jordan's gradient algorithm descends from the former, not from the latter.

## Missing or Suggested Additions

- Add a subsection separating:
  1. efficient universal QTM and precision robustness,
  2. BQP definition/containments,
  3. simple hidden-string BV algorithm,
  4. recursive Fourier sampling oracle separation.
