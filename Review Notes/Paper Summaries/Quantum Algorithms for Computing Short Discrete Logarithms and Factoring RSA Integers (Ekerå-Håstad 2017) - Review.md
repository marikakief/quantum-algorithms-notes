# Review: Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017)

Source note: [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]

## Primary Sources Checked

- Ekerå and Håstad, "Quantum algorithms for computing short discrete logarithms and factoring RSA integers," arXiv:1702.00249; PQCrypto 2017.

## Verdict

Minor issues. The note captures the core resource tradeoff and correctly warns that the paper counts exponent length rather than full fault-tolerant cost. The largest readability risk is the shift in the meaning of `n` between prime bit length and modulus bit length.

## Line-Anchored Comments

- Lines 13-35: clear problem statement. The no-wrap condition `r >= 2^{ell+m}+2^ell d` is not a side detail; it is the reason the RSA reduction needs a random large subgroup.
- Lines 41-49: good summary of the multi-run tradeoff. Say "quantum exponent length" rather than just "quantum cost" when reusing this paragraph.
- Lines 59-93: the sampling description is accurate. The output is not directly a congruence equation for `d`; it is a noisy/high-bit relation that becomes useful only after several good pairs.
- Lines 97-133: the lattice recovery is well explained. It would help to identify the final-coordinate check `x=[d]g` as the guard against wrong close lattice vectors.
- Lines 137-179: the RSA reduction is correct but probabilistic. The subgroup/base randomness and the condition on `t` should be mentioned in any headline statement about factoring RSA integers.
- Lines 209-221: the convention note is exactly right. Keep both conventions: `ell+m ~= (1+1/s)(n+1)` when `n` is prime length, and `(1/2+1/s)n` when `n` is modulus length.
- Lines 241-251: strong caveats. The fixed-`s` assumption matters because the classical subset/lattice post-processing constants are not harmless if `s` grows.

## Missing or Suggested Additions

- Add a one-line "not a new asymptotic class for factoring" caveat: it reduces the leading quantum arithmetic size inside Shor-style factoring, but does not change polynomial-time solvability.
- Cross-link this review to the Gidney-Ekerå resource estimate because that is where the asymptotic exponent-length saving becomes concrete engineering accounting.

