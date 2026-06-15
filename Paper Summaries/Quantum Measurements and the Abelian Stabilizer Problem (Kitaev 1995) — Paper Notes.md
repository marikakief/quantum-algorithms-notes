> **Source:** A. Yu. Kitaev, *Quantum measurements and the Abelian Stabilizer Problem*, arXiv:quant-ph/9511026 (1995); also in *Electronic Colloquium on Computational Complexity* (1996)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9511026)
> **Tags:** #phase-estimation #eigenvalue #quantum-fourier-transform #foundational #reversible-computation

---

## What the paper does

Introduces **quantum phase estimation** as a general measurement procedure for eigenvalues of unitary operators, and uses it to solve the Abelian Stabilizer Problem (which includes factoring and discrete log as special cases). This is an independent derivation of results equivalent to Shor's, using a fundamentally different conceptual framework — eigenvalue measurement rather than period finding.

The paper also introduces:
- The concept of **quantum measurements** as internal operations (one part of the computer measuring another)
- A general theory of reversible quantum computation (garbage-free)
- A polynomial quantum Fourier transform for arbitrary finite Abelian groups

This is one of the most important papers in quantum computing. The phase estimation procedure defined here became the backbone of quantum algorithms for eigenvalue problems, [[Hamiltonian simulation]] readout, and quantum chemistry — and it is a direct ancestor of the phase-estimation subroutines used in [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd]], [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]], and essentially every algorithm that needs to extract eigenvalue information from a unitary.

---

## The eigenvalue measurement procedure

### The core idea

Given a unitary operator $U$ with eigenstate $|\xi\rangle$ and eigenvalue $\lambda(\phi) = e^{2\pi i \phi}$, measure $\phi$ using controlled applications of $U$.

### Single-bit measurement (Lemma 9)

The operator $\Xi(U) = S \cdot \Lambda(U) \cdot S$ (where $S$ is the Hadamard gate and $\Lambda(U)$ is controlled-$U$) maps:

$$
|\xi, 0\rangle \mapsto |\xi\rangle \otimes \left(\frac{1+\lambda(\phi)}{2}|0\rangle + \frac{1-\lambda(\phi)}{2}|1\rangle\right)
$$

Measuring the ancilla gives outcome 1 with probability $\frac{1}{2}(1 - \cos 2\pi\phi)$.

By repeating $s$ times and counting 1's, you estimate $\frac{1}{2}(1 - \cos 2\pi\phi)$ to precision $\delta$ with error probability $\leq 2e^{-c(\delta)s}$.

Substituting $iU$ for $U$ gives $\sin 2\pi\phi$ as well. Together, $\phi$ is determined.

**Cost:** For a constant-additive-precision estimate of the single-bit cosine/sine experiment, confidence amplification to error probability $\leq \epsilon$ costs $O(\log(1/\epsilon))$ controlled-$U$ applications. This is not the full cost of obtaining an $l$-bit phase estimate.

### Multi-bit measurement (Lemma 10, Theorem 1)

To get precision $2^{-l-2}$: use the operator $U^{[0,r]}$ with $r = 2^l - 1$, which applies $U^a$ conditioned on register value $a$. Then measure $2^j \phi \pmod{1}$ for $j = 0, \ldots, l-1$ using $O(l \log(l/\epsilon))$ repetitions of Kitaev's measurement primitive, assuming coherent access to the controlled-power operation $U^{[0,r]}$. This is Kitaev's original measurement model, not the later textbook circuit cost model that separately counts controlled $U^{2^j}$ gates followed by an inverse QFT.

For a **permutation** $U$ on $N \subseteq B^n$ (eigenvalues are roots of unity with denominators $\leq 2^n$), the exact eigenvalue can be found using continued fractions from an approximation with precision $2^{-2n-1}$.

**Theorem 1:** Exact eigenvalue measurement with error $\leq \epsilon$ costs $\text{poly}(n) + O(n)\log(1/\epsilon)$ operations using $O(n\log(n/\epsilon))$ applications of $U^{[0, 2^{2n}]}$.

**Kitaev QPE vs textbook QPE:** Kitaev estimates eigenphases through repeated controlled-unitary measurement procedures. The now-standard single coherent register with controlled $U^{2^j}$ gates and an inverse QFT was crystallized and popularized later, especially by [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve-Ekert-Macchiavello-Mosca]].

---

## The Abelian Stabilizer Problem

**Definition:** Given a group action $F: \mathbb{Z}^k \times M \to M$ (where $M \subseteq B^n$) and an element $a \in M$, find the stabilizer $\text{St}_F(a) = \{g \in \mathbb{Z}^k : F(g, a) = a\}$.

**Special cases:**
- **Factoring / order finding:** For a fixed base $a \in (\mathbb{Z}/N\mathbb{Z})^\times$, the cyclic action $t \mapsto a^t \bmod N$ has stabilizer $r\mathbb{Z}$, where $r=\operatorname{ord}_N(a)$.
- **Discrete log:** With generator $\zeta$ and target $g=\zeta^s$ in a cyclic group, the action $(m,r)\mapsto \zeta^m g^r$ has a rank-one stabilizer encoding the relation $m+sr\equiv 0$ modulo the group order.

### Algorithm

The orbit $N = \{F(g, a) : g \in \mathbb{Z}^k\}$ carries a natural action of the quotient group $E = \mathbb{Z}^k / \text{St}_F(a)$. The Fourier basis of $\mathbb{C}(N)$ consists of eigenvectors:

$$
|\psi_h\rangle = \frac{1}{\sqrt{q}} \sum_{g \in E} e^{2\pi i (h, g)} |g(a)\rangle \qquad (h \in H = \text{Hom}(E, \mathbb{T}))
$$

Prepare the state $|a\rangle = \frac{1}{\sqrt{q}} \sum_h |\psi_h\rangle$ and measure $h$ using the eigenvalue measurement procedure. This gives a random element of $H$ with near-uniform distribution. After $l = n + 4$ independent samples, the stabilizer is determined with probability $\geq 2/3$.

**Total cost:** The blackbox $F$ is invoked $O(kn^2 \log(kn))$ times on inputs of size $O(n)$.

---

## Reversible quantum computation (Sections 2 and 5)

The paper develops a careful theory of reversible computation that is worth reading independently of the ASP application.

### The garbage problem

If a classical operator $G$ is computed via $U: (x, 0) \mapsto (G(x), g(x))$ where $g(x)$ is garbage, then tracing out the garbage destroys quantum coherence:

$$
\rho = \sum_{x,y: g(x)=g(y)} c_x^* c_y |G(x)\rangle\langle G(y)|
$$

If different inputs produce different garbage, $\rho$ becomes diagonal — equivalent to classical measurement. **Garbage makes quantum interference impossible.**

### Garbage-free computation (Lemma 1, 2)

**Lemma 1:** If $F: N \to B^m$ is computable by a circuit of size $L$, then the bijection $F^\tau: (u, v) \mapsto (u, v \oplus F(u))$ can be computed reversibly with $2L + m$ operations.

**Lemma 2 (the "compute-copy-uncompute" trick):** If both $G$ and $G^{-1}$ are computable by circuits of size $L, L'$, then $G$ can be computed reversibly (no garbage) with $2L + 2L' + 4n$ operations:

$$
(x, 0) \xrightarrow{G^\tau} (x, G(x)) \xrightarrow{\tau} (0, G(x)) \xrightarrow{(G^{-1})^\tau} (G(x), G(x)) \xrightarrow{\tau} (G(x), 0)
$$

### Theorem 2: Making quantum measurements reversible

If a unitary $U$ computes a function $F: \Omega \to \Theta$ with error $\leq \epsilon$, and $T$ is a measurement operator for $z_\Theta$, then $U^{-1} T U$ represents the reversible measurement $F^T$ with precision $2\sqrt{|\Omega|\epsilon}$.

This is what enables phase estimation to be used as a subroutine — the measurement is done reversibly, so the eigenvalue information can be processed further without destroying the eigenstate.

---

## Quantum Fourier Transform for arbitrary groups (Section 5)

Using reversible eigenvalue measurement + controlled phase gates, Kitaev constructs a polynomial QFT for any cyclic group $\mathbb{Z}_q$ (and thus any finite Abelian group):

$$
V_q |a\rangle = |\psi_{q,a}\rangle = \frac{1}{\sqrt{q}} \sum_{b=0}^{q-1} e^{2\pi i ab/q} |b\rangle
$$

Previous polynomial QFT algorithms existed only for $q = 2^n$ (Coppersmith) or smooth $q$ (Shor). Kitaev's construction works for arbitrary $q$.

---

## Reusable ideas

1. **Eigenvalue measurement via controlled-$U$ + Hadamard:** The $\Xi(U) = S \cdot \Lambda(U) \cdot S$ construction. Repeat $s$ times, estimate $\cos 2\pi\phi$ from the fraction of 1-outcomes. This is phase estimation in its most elementary form — before the QFT-based version standardized it.

2. **Reversible oracle evaluation (Lemma 1):** If $F$ is computed by a circuit of size $L$, the reversible map $(u,v)\mapsto(u,v\oplus F(u))$ costs $2L+m$ operations. This is the standard way to make a classical function coherently queryable.

3. **[[Reversible Computation via Compute-Copy-Uncompute|Garbage-free bijection implementation]] (Lemma 2):** If both a bijection $G$ and $G^{-1}$ are efficiently computable, then $G$ can be implemented without leftover garbage by computing, swapping/copying the output into place, and uncomputing with $G^{-1}$. This is related to but distinct from Lemma 1.

4. **Reversible quantum measurements (Theorem 2):** If $U$ approximately computes $F$, then $U^{-1} T U$ implements the measurement of $F$ reversibly. Error: $O(\sqrt{\epsilon})$. This makes phase estimation composable as a subroutine.

5. **Continued fractions for eigenvalue extraction:** Given an approximation to $p/q$ (with $q \leq 2^n$) to precision $2^{-2n-1}$, the exact fraction is recovered by Euclid's algorithm on the continued fraction expansion. Standard in phase estimation ever since.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring and discrete log via period-finding; this paper gives an alternative route via eigenvalue measurement
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — finding hidden subgroups; Kitaev generalizes Simon's procedure
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] and Deutsch (1989) — quantum Turing machines, circuits, and early quantum algorithms
- Yao (1993) — equivalence of quantum TMs and circuits
- Bennett (1973) — reversible computation (classical)
- Lecerf (1963) — reversible computation (classical, earlier)
- Barenco et al. (1995) — elementary gates for quantum computation
- Coppersmith (1994) — QFT for $q = 2^n$
- Grigoriev — shift equivalence problem (another ASP instance)

---

## Cross-links

### Paper notes
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]] — uses Kitaev's phase estimation for Hamiltonian eigenvalues
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL uses phase estimation for matrix inversion
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — modern eigenvalue filtering via [[QSVT Meta-Template|QSVT]], building on concepts from this paper
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] — uses phase estimation in the [[Hamiltonian-to-Projection via Phase Estimation|Hamiltonian-to-projection lemma]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — adiabatic approach to the same ground state problem

### Trick cards
- [[Iterative Phase Estimation (Kitaev)]] — later single-ancilla / semiclassical variants must be distinguished from Kitaev's original repeated-measurement procedure
- [[Gapped Phase Estimation]] — modern SOSSA-style interval classifier; not the default card for ordinary QPE
- [[Hamiltonian-to-Projection via Phase Estimation]]
- [[Reversible Computation via Compute-Copy-Uncompute]]

### Subsequent work
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] — reformulated QPE as a single circuit (controlled-$U^{2^k}$ + inverse QFT); unified Deutsch-Jozsa, BV, Shor as phase estimation instances
