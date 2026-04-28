# Approximate Encoded Permutation Framework

> **Source:** Craig Gidney, arXiv:1905.08488
> **Tags:** #trick #approximate #error-analysis #reversible-computation #quantum-arithmetic

## What it does

Provides composable error bounds for approximate quantum circuits built from reversible classical circuits that operate on many possible encodings of an input and send almost all encodings to an encoding of the correct output.

## The trick

An approximate encoded permutation is a tuple $(G, u, E, v, C, L, f)$: a desired permutation $u$ on a small set $G$ is realised by a cheap permutation $v$ on a larger encoded set $E$, with a reversible encoder $f : G \times C \to E$ parameterised by coset values $C$. The **deviation** is the maximum fraction of coset values that produce a wrong answer:

$$\text{Dev}(P) = \max_{g \in G} \frac{|\{c \in C : v(f(g,c)) \notin \text{Encodings}_{u(g)}\}|}{|C|}$$

In a quantum circuit, the coset register is prepared in uniform superposition, the encoded permutation $f^{-1} \circ v \circ f$ is applied, and the coset register is discarded. The key bound: deviation $\varepsilon$ implies trace distance at most $2\sqrt{\varepsilon}$ from the ideal output.

Deviation is subadditive under both **composition** (sequential operations: $\text{Dev}(P_0 \circ P_1) \leq \text{Dev}(P_0) + \text{Dev}(P_1)$) and **concatenation** (nested encodings: $\text{Dev}(P_0 * P_1) \leq \text{Dev}(P_0) + \text{Dev}(P_1)$). This lets you analyse long computation chains by summing individual deviations and applying the square-root trace distance bound once at the end.

## When to reach for it

- When you need to perform many operations on a quantum register (e.g., $n^2$ additions in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]) and an exact circuit is overkill — most of the circuit's work handles rare edge cases (like long carry chains).
- When you want to replace an expensive exact permutation (modular addition, modular multiplication) with a cheaper non-modular or piecewise version by encoding into a larger space.
- When you need to compose multiple approximate operations and bound the total error.

## Complexity

The framework itself adds no gates — it's an analysis tool. The cost is in the encoding/decoding circuits (application-specific) and the extra qubits for the coset register. For the two instantiations in the paper:
- [[Coset Representation for Approximate Modular Arithmetic]]: $m$ extra qubits, $O(nm)$ Toffoli encoding cost
- [[Oblivious Carry Runway]]: $m$ extra qubits per runway, $O(m)$ encoding cost per runway

## Caveat

The $2\sqrt{\varepsilon}$ bound relates deviation to trace distance, not diamond distance. For a single use this is fine, but when the approximate operation is used as a subroutine inside a larger quantum algorithm, the correct error metric may differ. The subadditivity results handle composition of approximate encoded permutations with each other, but not arbitrary interleaving with other quantum operations. In practice (e.g., Shor's algorithm), the register being operated on is entangled with other registers, and the trace distance bound still applies because it holds for all input states — but this requires care.

## Related notes
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — source paper
- [[Oblivious Carry Runway]] — instantiation for piecewise addition
- [[Coset Representation for Approximate Modular Arithmetic]] — instantiation for modular arithmetic
- [[Optimistic Quantum Circuits]] — a different approximate-circuit framework from [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes|Kahanamoku-Meyer et al. (2025)]], using measurement-and-retry rather than encoding redundancy
- [[Ancilla Opportunity Cost Analysis]] — relevant when evaluating whether the extra coset/runway qubits are worth it
