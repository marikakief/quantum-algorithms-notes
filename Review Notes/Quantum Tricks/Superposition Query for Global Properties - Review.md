# Review: Superposition Query for Global Properties

Source note: [[Superposition Query for Global Properties]]

## Primary Sources Checked

- Deutsch 1985 DOI/full text: https://doi.org/10.1098/rspa.1985.0070 and https://docslib.org/doc/7169585/quantum-theory-the-church-turing-principle-and-the-universal-quantum-computer
- Deutsch-Jozsa 1992 DOI: https://doi.org/10.1098/rspa.1992.0167
- Simon SIAM page: https://epubs.siam.org/doi/10.1137/S0097539796298637
- Shor arXiv: https://arxiv.org/abs/quant-ph/9508027
- Grover arXiv: https://arxiv.org/abs/quant-ph/9605043

## Verdict

Major issues. The card is pedagogically useful but its comparison table mixes query complexity, bit complexity, and number-theoretic runtime.

## Line-Anchored Comments

- Line 7: "Evaluates a function on a superposition of all inputs simultaneously"

  This is common shorthand, but the card itself warns against the misconception at line 38. Better opening: "Applies one coherent oracle call to a superposition of inputs, then uses interference..." That avoids implying all function values are independently available.

- Lines 19-26: comparison table

  The table mixes incompatible models. Deutsch-Jozsa, BV, Simon, and Grover are query-complexity rows. Shor's row cites number field sieve runtime, not classical query complexity for period finding. This is a serious review issue because it makes the speedups look commensurable when they are not.

- Line 25: "Shor ... Classical queries Omega(N^{1/3}) (number field sieve)"

  Incorrect category. Number field sieve is a bit-operation/runtime algorithm for factoring/discrete log, not a query lower bound for the period-finding oracle. Replace the "Classical queries" column with "Classical cost model" or split the table into oracle-query examples and concrete number-theory examples.

- Line 36: "Exponential speedups require algebraic structure"

  Good heuristic, but too strong as a theorem. There are other structured sources of exponential separations, and algebraic structure alone is not sufficient. Say "known standard exponential algorithmic speedups usually exploit strong structure such as periodicity, Fourier structure, or group actions."

## Missing or Suggested Additions

- Add a model column: exact query, bounded-error query, gate complexity, or bit complexity.
