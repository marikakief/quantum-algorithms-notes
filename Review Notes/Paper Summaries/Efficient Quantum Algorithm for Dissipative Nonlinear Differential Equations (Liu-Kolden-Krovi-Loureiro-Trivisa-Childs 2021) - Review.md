# Review: Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021)

Source note: [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]

## Primary Sources Checked

- Liu, Kolden, Krovi, Loureiro, Trivisa, and Childs, "Efficient quantum algorithm for dissipative nonlinear differential equations", PNAS 2021 / arXiv:2011.03185.

## Verdict

Minor issues. The Carleman construction, forward-Euler bottleneck, and lower-bound story are mostly accurate. The main fixes are to avoid "without loss of generality" language and to soften claims about nonlinear phenomena.

## Line-Anchored Comments

- Line 19: reducing higher-degree polynomial nonlinearities to quadratic is not "without loss of generality" for complexity. It can increase dimension, sparsity, coefficient norms, and stability parameters. Say it is a standard modeling reduction with overheads.
- Lines 23-25: good headline: this paper made the quantum Carleman line viable and also proved a meaningful large-`R` lower bound.
- Lines 47-51: the distinction between this paper's simple solution rescaling and later Carleman-vector rescaling is helpful.
- Lines 57-59: the measurement-probability formula is useful; keep `q` defined wherever it is used in tables.
- Lines 65-72: the theorem table is dense but valuable. The simplified row should state the extra assumption on the eigenvalues/diagonalizability clearly.
- Lines 80, 88, and 104: the measurement penalty is sometimes described as a probability and sometimes as a cost factor. Use one convention consistently, because `||u_in||^{2N}` and its inverse are easy to swap.
- Lines 92-96: strong lower-bound explanation. The gap `1 <= R < sqrt(2)` should be dated to the paper or "as of this note" rather than "as of 2025" unless you intend to maintain it.
- Line 110: "cannot capture bifurcations, chaos, or blowup" is too categorical. A truncated Carleman system can approximate nonlinear dynamics in its convergence regime; the proven efficient regime excludes many strongly nonlinear phenomena, which is the safer statement.

## Missing or Suggested Additions

- Add a one-sentence distinction between the Carleman truncation order and the physical/state dimension. The note uses `N`, `n`, and grid-point language across nearby summaries, and readers can easily conflate them.
