> **Source:** Harry Buhrman, Richard Cleve, John Watrous, and Ronald de Wolf, *Quantum fingerprinting*, Phys. Rev. Lett. **87**, 167902 (2001); arXiv:quant-ph/0102001
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0102001) · [PRL](https://doi.org/10.1103/PhysRevLett.87.167902)
> **Tags:** #communication-complexity #fingerprinting #exponential-gap #quantum-information #foundational

---

## The problem

**Simultaneous message passing (SMP) model:** Alice has $x \in \{0,1\}^n$, Bob has $y \in \{0,1\}^n$. They can't communicate with each other — they each send a message to a referee, who must determine if $x = y$.

**Classical communication complexity of equality in SMP:**
- With shared randomness: $O(1)$ bits (hash the string using the shared random key)
- Without shared randomness: $\Theta(\sqrt{n})$ bits (Newman-Szegedy 1996 lower bound)

**This paper:** $O(\log n)$ qubits suffice **without** shared randomness. An **exponential** quantum-classical gap.

---

## The quantum fingerprinting scheme

### Construction

Use a binary error-correcting code $E: \{0,1\}^n \to \{0,1\}^m$ with $m = cn$ and minimum distance $\geq (1-\delta)m$ between distinct codewords (e.g., Justesen codes with $\delta < 9/10 + 1/(15c)$).

The quantum fingerprint of $x$ is:

$$
|h_x\rangle = \frac{1}{\sqrt{m}} \sum_{i=1}^{m} |i\rangle|E_i(x)\rangle
$$

This is a $(\log m + 1)$-qubit state = $O(\log n)$ qubits.

**Key property:** For $x \neq y$:

$$
\langle h_x | h_y \rangle = \frac{|\{i : E_i(x) = E_i(y)\}|}{m} \leq \delta
$$

The inner product is at most $\delta$ because distinct codewords agree on at most $\delta m$ positions.

### The swap test (referee's protocol)

The referee receives $|h_x\rangle$ from Alice and $|h_y\rangle$ from Bob and applies the **controlled-swap (CSWAP) test**:

$$
(H \otimes I)(c\text{-SWAP})(H \otimes I)|0\rangle|\phi\rangle|\psi\rangle
$$

Measure the first qubit:
- If $|\phi\rangle = |\psi\rangle$: always get $|0\rangle$ (output "equal")
- If $|\langle\phi|\psi\rangle| \leq \delta$: get $|1\rangle$ (output "not equal") with probability $\frac{1 - |\langle\phi|\psi\rangle|^2}{2} \geq \frac{1-\delta^2}{2}$

One-sided error: never falsely rejects equal strings. Error probability for unequal strings reduced to $\epsilon$ by sending $k = O(\log(1/\epsilon))$ copies and repeating the test.

**Total communication:** $O(\log n \cdot \log(1/\epsilon))$ qubits each.

---

## Why classical simulation fails

A classical attempt: Alice sends $(i, E_i(x))$, Bob sends $(j, E_j(y))$ for random $i, j$. The referee can only compare when $i = j$, which happens with probability $O(1/n)$ — useless.

The quantum advantage: in the swap test, the referee computes $|\langle h_x|h_y\rangle|^2$ — a **global comparison** of the two fingerprints — even though each fingerprint is only $O(\log n)$ qubits. Classically, the referee can only compare individual bits, which is why $\Omega(\sqrt{n})$ communication is needed.

This is closely related to [[Superposition Query for Global Properties|superposition queries for global properties]] — the quantum fingerprints encode the entire codeword in superposition, and the swap test extracts a global comparison.

---

## Optimality and bounds

| Setting | Communication per party | Achievable? |
|---|---|---|
| Classical, shared randomness | $O(1)$ | Yes (hash) |
| Classical, no shared randomness | $\Theta(\sqrt{n})$ | Newman-Szegedy |
| **Quantum, no shared randomness** | $O(\log n)$ | **This paper** |
| Quantum lower bound | $\Omega(\log n)$ | Follows from $2^n$ distinct fingerprints in $2^b$ dimensions |

The $\Omega(\log n)$ lower bound: any $k$-qubit state can be specified with $O(k 2^k)$ classical bits, so a $k$-qubit quantum protocol implies an $O(k 2^k)$-bit classical protocol. Since classical deterministic requires $\Omega(n)$ bits, we need $k = \Omega(\log n)$.

---

## The state distinguishing problem

Section 4 optimises the distinguishing test: given $k$ copies of $|\phi\rangle$ and $k$ copies of $|\psi\rangle$, distinguish $|\phi\rangle = |\psi\rangle$ from $|\langle\phi|\psi\rangle| \leq \delta$.

**Optimal procedure:** Apply a random permutation to all $2k$ copies, then undo. Accept "equal" if the permutation register returns to $|0\rangle$.

**Error probability:** $\sim \sqrt{\pi k}\left(\frac{1+\delta}{2}\right)^{2k}$

**Lower bound (Helstrom):** Error $\geq \frac{1}{4}\left(\frac{1+\delta}{2}\right)^{2k}$

The procedure is optimal up to a $\sqrt{k}$ factor. This is a clean result on quantum state discrimination with multiple copies.

---

## Reusable ideas

1. **[[Quantum Fingerprinting via Error-Correcting Codes]]:** Encode $n$-bit strings as $O(\log n)$-qubit states using error-correcting codes: $|h_x\rangle = m^{-1/2}\sum_i |i\rangle|E_i(x)\rangle$. Distinct strings produce states with bounded inner product. Exponentially more compact than classical fingerprints without shared randomness.

2. **[[Swap Test for State Comparison]]:** Apply $H$, controlled-SWAP, $H$ to $|0\rangle|\phi\rangle|\psi\rangle$. Measuring the control qubit gives $|1\rangle$ with probability $(1 - |\langle\phi|\psi\rangle|^2)/2$. One-sided error, works for arbitrary unknown states. The core subroutine for fingerprint comparison, state certification, and inner product estimation.

---

## References within this paper

- Yao (1979) — introduced the SMP model of communication complexity; posed the question of public vs private coin
- Newman & Szegedy (1996) — $\Theta(\sqrt{n})$ classical lower bound for equality in SMP without shared randomness
- Ambainis (1996) — $O(\sqrt{n})$ classical upper bound
- Justesen (1972) — error-correcting codes used for the fingerprint construction
- Helstrom (1967) — optimal quantum state discrimination; implies the lower bound on the distinguishing test

---

## Cross-links

### Paper notes
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — another exponential quantum-classical gap (but for query complexity, not communication)
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — the conceptual origin of quantum advantages
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — same author (Watrous); group-theoretic quantum algorithms

### Trick cards
- [[Quantum Fingerprinting via Error-Correcting Codes]]
- [[Swap Test for State Comparison]]
- [[Superposition Query for Global Properties]] — the fingerprints encode global information in superposition
- [[Phase Kickback from Eigenstate Ancilla]]
