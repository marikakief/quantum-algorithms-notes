# Review: Primality Test Via Quantum Factorization (Chau-Lo 1996)

Source note: [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]

## Primary Sources Checked

- Chau and Lo, "Primality Test Via Quantum Factorization," arXiv:quant-ph/9508005; Int. J. Mod. Phys. C 8(2), 1997.

## Verdict

Minor issues. The note is historically and technically well positioned. It correctly treats the algorithm as a primality-proving/certificate construction rather than as the modern best primality test.

## Line-Anchored Comments

- Lines 11-21: good framing. The phrase "may not halt on composite" is acceptable for the Pocklington-prover branch, but should be paired with the note's own caveat that Shor factoring gives a separate compositeness/factoring route.
- Lines 23-37: the Pocklington-Lehmer certificate statement is clear. If this is quoted elsewhere, mention that complete factorization of `N-1` and primality of those factors are part of the recursive certificate chain.
- Lines 41-49: the historical comparison to Donis-Vela--Garcia-Escartin is useful. The important distinction is classical certificate after quantum work versus direct order-finding evidence.
- Lines 53-78: the handling of prime powers is a nice detail and should stay; it prevents a misleading "just call Shor recursively" gloss.
- Lines 101-122: the first witness-search cost is explained well. The `O(1/log log N)` success probability is paper-specific accounting; avoid treating it as a standard Pocklington fact without this context.
- Lines 123-172: good explanation of how the extra `log log N` is removed.
- Lines 208-215: the comparison table is good, but the APRCL row should remain historical and cautious. Modern AKS/ECPP context belongs in the caveats, not in the original-paper comparison.

## Missing or Suggested Additions

- Add a status sentence: "pre-AKS; mainly interesting as a quantum route to classical primality certificates."
- Add a short note that the recursive factor-primality proof is separate from merely factoring `N-1`; otherwise the certificate condition can look non-recursive.

