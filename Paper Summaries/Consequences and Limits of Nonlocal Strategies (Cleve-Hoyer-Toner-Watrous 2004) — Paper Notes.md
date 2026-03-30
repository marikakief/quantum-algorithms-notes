> **Source:** Richard Cleve, Peter Høyer, Benjamin Toner, John Watrous, *Consequences and Limits of Nonlocal Strategies*, CCC 2004; arXiv:quant-ph/0404076
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0404076) · [CCC](https://doi.org/10.1109/CCC.2004.1313847)
> **Tags:** #nonlocal-games #bell-inequality #interactive-proofs #entanglement #MIP-star #foundational

---

## The computational problem

**Nonlocal games:** A referee draws questions $(s,t)$ from $S \times T$ according to distribution $\pi$, sends $s$ to Alice and $t$ to Bob. Without communicating, they respond with $a \in A$ and $b \in B$. They win if $V(a,b \mid s,t) = 1$.

Two values matter:
- **Classical value** $\omega_c(G)$: best winning probability using shared randomness (equivalently, deterministic strategies)
- **Quantum value** $\omega_q(G)$: supremum over all quantum strategies (shared entangled state + local measurements)

The gap $\omega_q(G) - \omega_c(G)$ captures exactly how much entanglement can help — and how much Bell inequality violations can occur.

---

## What the paper does

This is the paper that formalises nonlocal games as a computational/complexity-theoretic framework, connecting three previously separate threads: Bell inequality violations (physics), multi-prover interactive proofs (complexity theory), and entangled strategies (quantum information). It provides:

1. Four concrete games where quantum beats classical (CHSH, Odd Cycle, Magic Square, Kochen-Specker), presented cleanly as nonlocal games
2. Two natural two-prover proof systems (graph colouring, 3-SAT) that become unsound against entangled provers — introducing the MIP vs. MIP* question
3. Systematic upper bounds on quantum values of binary and XOR games, connecting to Tsirelson's inequality, Grothendieck's constant, and semidefinite programming
4. Bounds on entanglement needed for optimal/near-optimal strategies

The conceptual contribution matters more than any single theorem. This paper established the language and framework that the entire field of device-independent quantum information and ultimately the MIP* = RE result built upon.

---

## The four example games

### CHSH game
$S = T = A = B = \{0,1\}$, uniform $\pi$. Win if $a \oplus b = s \wedge t$.

$$\omega_c = \frac{3}{4}, \qquad \omega_q = \cos^2\!\left(\frac{\pi}{8}\right) \approx 0.854$$

Optimal quantum strategy: share $|\Phi^+\rangle = (|00\rangle + |11\rangle)/\sqrt{2}$, measure in bases rotated by $0, \pi/4$ (Alice) and $\pi/8, -\pi/8$ (Bob). Optimality follows from Tsirelson's inequality.

### Odd Cycle game
$n$-vertex odd cycle. Alice and Bob get same or adjacent vertices, must consistently 2-colour. Classical value $1 - 1/2n$, quantum value $\cos^2(\pi/4n) \geq 1 - O(1/n^2)$. The quantum strategy is quadratically closer to perfect. The paper proves this is optimal (Corollary 9).

### Magic Square game
Based on Mermin-Peres-Aravind: no 3×3 binary matrix has even-parity rows and odd-parity columns. Using Pauli observables on two copies of $|\psi^-\rangle$, Alice and Bob win with probability 1. Classically, $\omega_c = 17/18$.

This is a "pseudo-telepathy" game: perfect quantum strategy, imperfect classical. It directly breaks the 3-SAT oracularisation paradigm (Section 4.2).

### Kochen-Specker game
Based on Kochen-Specker theorem. Uses a qutrit maximally entangled state $|\psi\rangle = (|00\rangle + |11\rangle + |22\rangle)/\sqrt{3}$. Perfect quantum strategy exists; no perfect classical strategy (by KS theorem).

---

## Breaking multi-prover interactive proofs

This is where the paper connects to complexity theory. Two natural proof systems become unsound against entangled provers:

**Graph Colouring:** Verifier asks each prover for the colour of a vertex (same or adjacent). If the graph is $k$-colourable, honest provers win. If not, classical provers fail with probability $\geq 1/(|V| + |E|)$. But for certain non-$k$-colourable graphs (based on Frankl-Rödl / Brassard-Cleve-Tapp), entangled provers win with probability 1.

**3-SAT:** Verifier sends Alice a clause, Bob a variable from that clause. For unsatisfiable formulas, classical provers fail. But the Magic Square game provides a 24-clause unsatisfiable formula where entangled provers win perfectly — breaking the Fortnow-Rompel-Sipser oracularisation paradigm.

These examples motivated the definition of MIP* (multi-prover interactive proofs with entanglement). The paper poses the question: does MIP* $=$ NEXP? The answer turned out to be far more dramatic — Ji-Natarajan-Vidick-Wright-Yuen (2020) proved MIP* $=$ RE, showing entangled provers are strictly more powerful than unentangled ones.

---

## Key results

**Theorem 3 (Binary game perfect strategies):** If $G$ is a binary game (both answers in $\{0,1\}$) with $\omega_q(G) = 1$, then $\omega_c(G) = 1$. Perfect quantum strategies for binary games can always be derandomised classically. (Fails for ternary games — see Kochen-Specker.)

**Theorem 7 (Grothendieck bound for XOR games):**

$$\omega_q(G) - \tau(G) \leq K_G \cdot [\omega_c(G) - \tau(G)]$$

where $K_G \in [1.677, 1.782]$ is Grothendieck's constant and $\tau(G)$ is the trivial random strategy's success probability. The quantum advantage for XOR games is bounded by a universal constant factor.

**Theorem 8 (Quadratic improvement bound for XOR games):**

$$\omega_q(G) \leq \sin^2\!\left(\frac{\pi}{2} \omega_c(G)\right) \quad \text{for } \omega_c(G) > \gamma_2 \approx 0.742$$

The quantum failure probability can improve at most quadratically over classical for XOR games. This gives the optimality of the Odd Cycle quantum strategy.

**Theorem 10 (Entanglement bound):** For an XOR game with $m = \min(|S|, |T|)$, there exists an optimal strategy using a maximally entangled state on $\lceil m/2 \rceil$ qubits. With Johnson-Lindenstrauss dimensionality reduction, an $\varepsilon$-near-optimal strategy uses $O(\log(|S| + |T|)/\varepsilon^2)$ qubits (Theorem 12).

**Corollary 16:** $\oplus\text{MIP}^*_{c,s}[2] \subseteq \text{EXP}$ for all $0 \leq s < c \leq 1$. XOR proof systems with entangled provers are contained in EXP, not NEXP. (Contrast with $\oplus\text{MIP}_{c,s}[2] = \text{NEXP}$ from Håstad.)

---

## Comparison with prior and subsequent work

| Aspect | This paper | Prior | Subsequent |
|---|---|---|---|
| CHSH game analysis | Clean game-theoretic formulation | Bell (1964), CHSH (1969) as physics thought experiments | — |
| MIP vs MIP* question | Posed here; showed MIP proof breaks with entanglement | MIP = NEXP (Babai-Fortnow-Lund) | MIP* = RE (JNVWY 2020) |
| XOR game bounds | Grothendieck + quadratic improvement | Tsirelson (1980, 1987) | Kempe-Regev-Toner (2008): unique games with entangled provers are easy |
| Perfect binary game result | $\omega_q = 1 \Rightarrow \omega_c = 1$ for binary | — | Extended to general games in various ways |

---

## Limits / caveats

- The framework covers only one-round two-prover games. Multi-round protocols require additional machinery (and the MIP* = RE proof needs it).
- The quantum value $\omega_q(G)$ is defined as a supremum — it's not known whether it's always achieved (the dimension $n$ is unbounded). This subtlety proved central to MIP* = RE.
- The upper bounds (Theorems 7, 8) apply only to XOR and binary games, not general games. General nonlocal games are much harder to bound.
- The paper's graph colouring counterexample needs 32,768 vertices. The Magic Square counterexample is much cleaner (24 clauses).
- The erratum (Section 1, remark) corrects a false lemma from the original 2004 version; the current version (2010) has the corrected proof of Theorem 3.

---

## Reusable ideas

1. **[[Nonlocal Game Framework]]:** Formalise any two-party correlation scenario as a nonlocal game $(V, \pi)$ with classical value $\omega_c$ and quantum value $\omega_q$. The gap $\omega_q - \omega_c$ is a clean measure of quantum advantage. Computing $\omega_q$ for XOR games reduces to semidefinite programming (via Tsirelson's theorem); computing $\omega_c$ for XOR games is NP-hard. This framework turned out to be the right abstraction for everything from device-independent cryptography to the complexity of entanglement.

2. **[[Tsirelson Correspondence for XOR Games]]:** For XOR games, quantum correlations $\langle\psi| A_s \otimes B_t |\psi\rangle$ are exactly characterised by inner products $\langle u_s | v_t \rangle$ of real unit vectors (Tsirelson's theorem). This converts quantum value computation to a semidefinite program. The classical value is the integer relaxation — the quantum/classical gap is bounded by Grothendieck's constant $K_G$.

3. **[[Magic Square Pseudo-Telepathy]]:** The $3 \times 3$ Pauli observable matrix (commuting within rows/columns, product $+I$ for rows, $-I$ for columns) on two copies of $|\psi^-\rangle$ produces deterministic outcomes satisfying contradictory parity constraints. This is the simplest proof that entanglement produces correlations impossible classically, and the cleanest counterexample to oracularisation.

---

## References within this paper

- Bell (1964) — the original nonlocality paper; entangled particles produce correlations violating local hidden variable bounds
- CHSH (1969) — Clauser-Horne-Shimony-Holt inequality; the canonical Bell inequality
- Tsirelson (1980, 1987) — quantum bound on Bell inequality violation ($2\sqrt{2}$ for CHSH); the correspondence between quantum correlations and real vectors
- Mermin (1990, 1993) — Mermin-Peres square; no-hidden-variables theorems
- Kochen-Specker (1967) — contextuality theorem; the basis for the KS game
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes|Buhrman-Cleve-Wigderson (1998)]] — referenced for the Frankl-Rödl intersection result used in graph colouring counterexample
- Babai-Fortnow-Lund (1991) — NEXP ⊆ MIP
- Feige-Lovász (1992) — MIP = NEXP with one round
- Fortnow-Rompel-Sipser (1994) — oracularisation paradigm (which the Magic Square breaks)
- Kobayashi-Matsumoto (2003) — bounded entanglement implies MIP* ⊆ NEXP
- Håstad (2001) — optimal PCP inapproximability results; the XOR proof system for NEXP
- Grothendieck (1953) — the constant $K_G$ bounding bilinear forms; connects to quantum XOR advantage
- Goemans-Williamson (1995) — the rounding technique (random hyperplane) used in Theorem 8's proof is exactly the Goemans-Williamson MAX-CUT rounding

---

## Cross-links

### Paper notes
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]] — Buhrman-Cleve-Wigderson's communication separations; uses the same Frankl-Rödl result
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]] — same authors (Cleve, Watrous); communication complexity perspective
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]] — Watrous's work on quantum interactive proofs (single prover); related complexity-theoretic questions

### Trick cards
- [[Nonlocal Game Framework]]
- [[Tsirelson Correspondence for XOR Games]]
- [[Magic Square Pseudo-Telepathy]]
- [[Black-Box to Communication Reduction]] — Buhrman-Cleve-Wigderson's reduction, which this paper's graph colouring example builds on
