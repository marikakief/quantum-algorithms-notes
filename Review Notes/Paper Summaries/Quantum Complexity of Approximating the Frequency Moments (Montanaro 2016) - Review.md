# Review: Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016)

Source note: [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]

## Verdict

Minor-to-major issues. The algorithms and resource tradeoffs are described well. The key correction is that the `F_0` upper and lower bounds in epsilon are not presented consistently; the note calls a bound tight even though the displayed formulas do not match.

## Line-Anchored Comments

- Lines 7-40: The problem statement is clear. Add whether `m` is assumed polynomially related to `n` in each streaming theorem, because universe-size dependence matters.

- Lines 42-46: The assessment correctly says the honest streaming comparison is `TS`, not only space. Keep this.

- Lines 50-76: The hash-minima estimator is well summarized. Clarify that the quantum algorithm must find the `d` smallest distinct hash values, not merely the `d` smallest positions.

- Lines 78-124: The `F_k` query algorithm is dense but useful. Add a short intuition for why Belovs' `k`-distinctness exponent appears.

- Lines 213-219 and 235-243: The note says the `F_0` upper bound `O(sqrt(n)/epsilon)` is tight, but the displayed lower bound is `Omega(sqrt(n/epsilon))`. Either the lower bound or the tightness claim needs correction; at minimum say "tight in `n` for constant epsilon."

- Lines 255-302: The streaming theorem list is good. Include passes and qubits together in the comparison lines to avoid overselling a space-only improvement.

- Lines 315-321: The caveats are strong. The possible `1/epsilon` improvement for `F_k` should be flagged as speculative/mentioned by Montanaro, not an achieved result.

## Missing or Suggested Additions

- Add a table for access model: query, streaming with passes, and what is counted as memory.
- Add one sentence explaining why coherent oracle access is stronger than ordinary streaming access.
