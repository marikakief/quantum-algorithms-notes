# Review: Reversible Modular Exponentiation with Garbage Cleanup

Source note: [[Reversible Modular Exponentiation with Garbage Cleanup]]

## Primary Sources Checked

- Shor, "Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer", arXiv: https://arxiv.org/abs/quant-ph/9508027
- Beauregard, "Circuit for Shor's algorithm using 2n+3 qubits", arXiv: https://arxiv.org/abs/quant-ph/0205095

## Verdict

Major issues. The reversible cleanup idea is right, but the card attributes a "quantum watchdog" error-detection story to Shor in a way that is likely to mislead.

## Line-Anchored Comments

- Lines 7 and 11-20: modular exponentiation skeleton

  Correct as a high-level repeated-squaring decomposition: the constants `x^{2^i} mod N` are classically precomputed and controlled multiplications are applied according to exponent bits.

- Lines 22-31: cleanup via inverse multiplication

  Correct in spirit, especially for Beauregard/HRS-style modular multiplication by known invertible constants. The card should distinguish "copy result then uncompute" from the more space-efficient swap-and-multiply-by-inverse trick used in small-qubit factoring circuits.

- Lines 24-29: notation

  The card says the forward computation produces `(a, b, x^a mod N)` where `b` is garbage, but then uses `c^{-1}` without defining `c` clearly. Use a concrete reversible multiplier example: compute `|x>|0> -> |x>|a x mod N>`, swap, then multiply by `a^{-1} mod N` to restore the old register to zero.

- Line 33: "Shor's quantum watchdog"

  This should be removed or heavily softened. Standard reversible uncomputation is not the same as mid-circuit fault-tolerant error detection. Measuring an alleged garbage register after uncomputation can detect some faults in an idealized circuit, but it is not a syndrome-extraction or "watchdog" mechanism in the fault-tolerance sense and is not a central Shor result.

- Lines 43-45: complexity

  Needs parameter conventions. If `n = ceil(log_2 N)`, schoolbook modular exponentiation is commonly `O(n^3)` gates. Faster multiplication changes asymptotics, but reversible implementation overhead and cleanup must be stated rather than quoted as classical multiplication cost alone.

## Missing or Suggested Additions

- Add a "requires `gcd(x,N)=1`" check at the top. In factoring, if the chosen base is not coprime to `N`, the classical gcd has already found a factor and the quantum order-finding branch is unnecessary.
