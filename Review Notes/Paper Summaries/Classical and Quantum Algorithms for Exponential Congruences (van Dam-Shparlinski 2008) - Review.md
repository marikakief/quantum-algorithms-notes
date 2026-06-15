# Review: Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008)

Source note: [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]]

## Primary Sources Checked

- van Dam and Shparlinski, "Classical and Quantum Algorithms for Exponential Congruences," arXiv:0804.1109; TQC 2008.

## Verdict

Minor issues. The note is careful about the main trap: the speedups are polynomial in `q`, while the input length is `log q`. The main improvements are to keep the HSP motivation separated from the achieved runtime and to preserve the quantifiers in the typical-case theorems.

## Line-Anchored Comments

- Lines 11-25: good setup. The input-length warning at line 25 is essential and should remain close to the first displayed runtime.
- Lines 29-41: the high-level comparison is accurate. "Cubic quantum speedup" is defensible relative to the presented `q^{9/8}` deterministic classical bound, but should not be read as a lower-bound separation.
- Lines 55-80: the character-sum window lemma is summarized well. The guarantee is conditional on the window fitting inside the smaller order, so `r <= t` should stay visible when the lemma is cited later.
- Lines 102-109: good accounting of the two quantum ingredients. The predicate is not just membership in a subgroup; it also recovers the discrete logarithm when the membership test succeeds.
- Lines 111-115: the "almost all `c`" qualifier is important. The typical-case result averages over the right-hand side, not over arbitrary cryptographic instance generation.
- Lines 163-180: the large-order theorem is stated correctly. The "at most `q^{1/8}`" consequence follows only under the threshold on `st`; do not quote it as an unconditional runtime.
- Lines 216-224: strong caveats. The HSP-relevant special case still costs about `q^{1/4}` in the note's notation, so this paper is a partial attack on the number-theoretic subproblem, not an efficient semidirect-product HSP algorithm.

## Missing or Suggested Additions

- Add a small parameter table with `q`, `s`, `t`, `r`, and which quantity is being searched in each theorem.
- State explicitly that the classical comparison is to the methods in the paper, not to all later finite-field discrete-log algorithms in every field family.

