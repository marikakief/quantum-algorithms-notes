# Amplitude Amplification and Estimation

**Concept hub** — 42 wikilink refs across the vault.

---

## What it is

**Amplitude amplification** generalises Grover search: given a unitary $A$ that prepares a "good" state with amplitude $a$, the algorithm boosts success probability to near 1 using $O(1/a)$ applications of $A$ and its inverse — quadratically better than classical repetition.

**Amplitude estimation** extracts the value of $a$ (or a function thereof) to precision $\varepsilon$ using $O(1/\varepsilon)$ calls, via phase estimation on the Grover-like operator $Q = -AS_0A^{-1}S_\chi$.

Both are foundational primitives: they underlie quantum Monte Carlo speedups, quantum linear systems solvers, and the runtime of most query-complexity-optimal quantum algorithms.

---

## Primary reference

- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp 2002]] — the foundational paper

---

## Related techniques and applications in this vault

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover 1996]] — original search algorithm
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr & Høyer 1996]] — minimum-finding via amplitude amplification
- [[Hamiltonian simulation]] — amplitude amplification used inside walk-based simulation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang 2019]] — qubitization uses QSP in place of standard AA

---

## See also

- [[Amplitude Amplification and Estimation]] (this page)
- [[Hamiltonian simulation]]
