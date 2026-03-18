> **Source:** Gilles Brassard, Peter Høyer, Michele Mosca, and Alain Tapp, *Quantum Amplitude Amplification and Estimation*, Contemporary Mathematics **305**, AMS (2002); arXiv:quant-ph/0005055
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0005055)
> **Tags:** #amplitude-amplification #amplitude-estimation #grover #counting #foundational

---

## What the paper does

Generalizes Grover's search algorithm into two tools that became ubiquitous in quantum algorithms:

1. **Amplitude amplification:** Given any quantum algorithm $A$ that produces a good state with probability $a$, boost the success probability to near 1 using $O(1/\sqrt{a})$ applications of $A$ and $A^{-1}$. Grover search is the special case where $A$ is the Hadamard transform.

2. **Amplitude estimation:** Estimate the success probability $a$ itself, to additive error $\epsilon$, using $O(1/\epsilon)$ applications of $A$. This combines amplitude amplification with [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]].

---

## Amplitude amplification

### Setup

Let $A$ be a quantum algorithm (no intermediate measurements) with $A|0\rangle = |\Psi\rangle$. A Boolean function $\chi$ partitions the Hilbert space into good ($\chi(x) = 1$) and bad ($\chi(x) = 0$) subspaces. Write:

$$
|\Psi\rangle = |\Psi_1\rangle + |\Psi_0\rangle, \qquad a = \langle\Psi_1|\Psi_1\rangle
$$

### The operator

$$
Q = -A S_0 A^{-1} S_\chi
$$

where $S_\chi$ flips the sign of good states and $S_0$ flips the sign of $|0\rangle$.

### The 2D subspace structure

$Q$ acts on the 2D subspace spanned by $|\Psi_1\rangle$ and $|\Psi_0\rangle$ as a rotation by $2\theta_a$ where $\sin\theta_a = \sqrt{a}$:

$$
Q^j|\Psi\rangle = \sin((2j+1)\theta_a)|\Psi_1'\rangle + \cos((2j+1)\theta_a)|\Psi_0'\rangle
$$

where $|\Psi_1'\rangle = |\Psi_1\rangle/\sqrt{a}$ and $|\Psi_0'\rangle = |\Psi_0\rangle/\sqrt{1-a}$ are normalized.

After $m = \lfloor \pi/(4\theta_a) \rfloor$ iterations, the success probability is $\sin^2((2m+1)\theta_a) \geq 1 - a$.

**Key result (Theorem 2):** If $a$ is known, a good state is found with certainty after $\lfloor \pi/(4\theta_a)\rfloor$ applications of $Q$, i.e., $O(1/\sqrt{a})$ uses of $A$ and $A^{-1}$.

**When $a$ is unknown (Theorem 3):** Run with exponentially increasing guesses for $m$. Expected number of applications of $A$: $O(1/\sqrt{a})$.

---

## Amplitude estimation

### The idea

The operator $Q$ has eigenvalues $e^{\pm 2i\theta_a}$ on the 2D subspace. Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to $Q$ to extract $\theta_a$, and thus $a = \sin^2\theta_a$.

### Precise result (Theorem 12)

Using $M$ applications of $Q$ (and its powers), the estimate $\tilde{a}$ satisfies:

$$
|\tilde{a} - a| \leq \frac{2\pi\sqrt{a(1-a)}}{M} + \frac{\pi^2}{M^2}
$$

with probability $\geq 8/\pi^2 \approx 0.81$.

### Application: approximate counting

Given $f: \{0, \ldots, N-1\} \to \{0,1\}$ with $t$ satisfying elements:

| Task | Cost (evaluations of $f$) |
|---|---|
| Estimate $t$ within $\sqrt{t}$ | $O(\sqrt{N})$ |
| Estimate $t$ within $\epsilon t$ | $O(\sqrt{N/t}/\epsilon)$ |
| Decide $t = 0$ vs. $t = t_0$ | $O(\sqrt{N/t_0})$ (exact, with certainty) |
| Exact count | $O(\sqrt{t(N-t)})$ with high probability |

These are optimal — matching lower bounds from polynomial method.

---

## Generalizations from classical heuristics (Section 3)

If a classical heuristic algorithm $\mathcal{A}$ finds a good element in expected time $T$, the quantized version finds it in time $O(\sqrt{T})$. This applies to:
- Las Vegas algorithms (always correct, random runtime)
- Randomized search with restart strategies
- Markov chain heuristics

**Theorem 6:** If the classical heuristic uses $T$ steps on average, amplitude amplification finds a good element in $O(\sqrt{T})$ quantum steps.

This is the generic "quadratic speedup for search" result. It's the reason amplitude amplification shows up everywhere — any classical search subroutine can be quantumly sped up by the square root.

---

## Relationship to later work

| This paper (2002) | Later development |
|---|---|
| Standard amplitude amplification (reflects about $A\|0\rangle$) | [[Oblivious Amplitude Amplification (Robust)\|Oblivious AA]] (Berry et al. 2014): reflects about ancilla $\|0^\mu\rangle$ only, works for any input |
| Amplitude estimation via phase estimation on $Q$ | [[Kaiser-Window Amplitude Estimation\|Kaiser-window AE]]: better spectral leakage control |
| Quadratic speedup for search | [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes\|Szegedy (2004)]]: $\sqrt{\delta\epsilon}$ rule for walk-based search |
| $Q = -AS_0 A^{-1} S_\chi$ | [[QSVT Meta-Template\|QSVT]]: general polynomial of singular values, amplitude amplification as special case |

---

## Reusable ideas

1. **[[Standard Amplitude Amplification]]:** $Q = -AS_0 A^{-1}S_\chi$ rotates by $2\theta_a$ in the good/bad 2D subspace. $O(1/\sqrt{a})$ iterations to find a good state. The foundation for all quantum search speedups.

2. **[[Amplitude Estimation via Phase Estimation on Q]]:** Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to $Q$ to extract $\theta_a = \arcsin\sqrt{a}$. Cost: $O(1/\epsilon)$ for additive error $\epsilon$. Converts search oracles into counting oracles.

3. **Quadratic speedup for classical heuristics:** Any classical randomized algorithm with expected runtime $T$ can be quantumly accelerated to $O(\sqrt{T})$. This is a black-box result — works without understanding the heuristic's internal structure.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996, 1997)]] — original database search; amplitude amplification is the generalization
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — [[Gapped Phase Estimation|phase estimation]] used for amplitude estimation
- Brassard & Høyer (1997) — earlier version of amplitude amplification
- Boyer, Brassard, Høyer & Tapp (1998) — tight bounds for search with unknown number of solutions
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett, Bernstein, Brassard & Vazirani (1997)]] — $\Omega(\sqrt{N})$ lower bound for search; Grover is optimal
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard, Høyer & Tapp (1997)]] — collision finding via [[Classical Preprocessing plus Grover Search|classical table + Grover]], a precursor application of amplitude amplification

---

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — extends amplitude amplification to walk-based search
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — uses amplitude amplification to boost $1/\kappa^2$ success probability
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — [[QSVT Meta-Template|QSVT]] subsumes amplitude amplification as a polynomial transform
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — uses amplitude amplification for ground state extraction

### Trick cards
- [[Standard Amplitude Amplification]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Oblivious Amplitude Amplification (Robust)]] — the "input-oblivious" generalization
- [[Variable-Time Amplification for Condition-Number Reduction]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
- [[Kaiser-Window Amplitude Estimation]]
