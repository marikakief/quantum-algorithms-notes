
**Concept stub** — referenced in Exponential Algorithmic Speedup by Quantum Walk (Childs et al. 2003).

---

## What it is

If you can simulate $H$ and implement a unitary $U$ efficiently, you can simulate $UHU^\dagger$ with no extra asymptotic cost: simply conjugate the $H$-simulation circuit by $U$ and $U^\dagger$.

Formally: $e^{-i(UHU^\dagger)t} = U \, e^{-iHt} \, U^\dagger$.

This lets you "change basis" in [[Hamiltonian simulation]] for free — useful when $H$ is sparse or diagonal in the computational basis but the problem is naturally expressed in a different basis.

---

## Where it appears

- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs et al. 2003]] — listed as a primitive simulation technique (alongside direct simulation, [[product formula]]s, and Taylor series)
- Implicitly used throughout [[Hamiltonian simulation]] whenever a block-encoding is conjugated by a Clifford or other efficiently-implementable unitary

---

## See also

- [[Hamiltonian simulation]]
- [[product formula]]
