> **Source:** Richard Cleve, Artur Ekert, Chiara Macchiavello, Michele Mosca, *Quantum Algorithms Revisited*, Proc. R. Soc. Lond. A 454, 339–354, 1998. arXiv:quant-ph/9708016
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9708016) · [Proc. R. Soc.](https://doi.org/10.1098/rspa.1998.0164)
> **Tags:** #phase-estimation #phase-kickback #QFT #survey #foundational

---

## What this paper does

Provides a unified framework for understanding many then-known quantum algorithms as **multi-particle interferometry** and phase readout. This is a review/unification paper, not the original invention of phase estimation. The paper:

1. Reformulates Deutsch's, Deutsch-Jozsa's, and Bernstein-Vazirani's algorithms as phase kickback from controlled-$U$ operations on eigenstates
2. Gives a clean derivation of the quantum Fourier transform circuit (the standard one everyone uses)
3. Gives a clean explicit circuit presentation of **phase estimation**, building on Kitaev's earlier eigenvalue-measurement procedure
4. Shows that Shor's factoring algorithm is phase estimation applied to the modular exponentiation operator
5. Constructs a universal method for generating arbitrary interference patterns

This is the paper that crystallised the "phase estimation" view of quantum algorithms, building on [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]]. The phase estimation algorithm as typically taught in textbooks (controlled-$U^{2^k}$ followed by inverse QFT) is essentially the version presented here.

---

## The unifying framework

### The kernel: phase kickback + interference

Every algorithm follows the same pattern:

1. **Hadamard/Fourier transform** on the control register → creates superposition of inputs
2. **Controlled-$U$** operation → eigenphase kicks back into the control register
3. **Inverse Fourier transform** on the control register → extracts phase information via interference

The controlled-$U$ step is the computational work; everything else is the interferometric apparatus for reading out the result.

**Key observation (Eq. 1.2):** If $U|u\rangle = e^{i\phi}|u\rangle$, then:

$$|0\rangle|u\rangle \xrightarrow{H} \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)|u\rangle \xrightarrow{c\text{-}U} \frac{1}{\sqrt{2}}(|0\rangle + e^{i\phi}|1\rangle)|u\rangle \xrightarrow{H} \left(\cos\frac{\phi}{2}|0\rangle - i\sin\frac{\phi}{2}|1\rangle\right)e^{i\phi/2}|u\rangle$$

This is the quantum analogue of a Mach-Zehnder interferometer, where $\phi$ is the relative phase shift between the two paths.

### Algorithms as special cases

| Algorithm | What plays the role of $U$ | Eigenphase $\phi$ |
|---|---|---|
| [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] | $U_f: |y\rangle \to |y \oplus f(x)\rangle$ | $\pi f(x)$ |
| Deutsch-Jozsa (1992) | Same, $n$-bit version | $\pi f(x)$ for each $x$ |
| Bernstein-Vazirani (1993) | $f(x) = s \cdot x$ | $\pi(s \cdot x)$ |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] | $U: |x\rangle \to |ax \bmod N\rangle$ | $2\pi k/r$ (order $r$) |

---

## The Quantum Fourier Transform circuit

The paper gives the standard QFT network (Fig. 5), with the key insight that the QFT of a computational basis state factors into single-qubit states:

$$
F_{2^m}|a_1\ldots a_m\rangle
= \bigotimes_{k=1}^{m}
\frac{|0\rangle + e^{2\pi i(0.a_{m-k+1}\ldots a_m)}|1\rangle}{\sqrt{2}}
$$

This product structure is why the QFT can be computed with $O(m^2)$ gates: each qubit gets a Hadamard plus controlled-$R_k$ rotations that build up the binary fraction in the exponent.

The circuit uses $R_k = \text{diag}(1, e^{2\pi i/2^k})$ gates, controlled on subsequent qubits. This is the construction that appears in every textbook and is used in all QFT-based algorithms.

---

## Phase estimation (Section 5)

### The algorithm

Given:
- A unitary $U$ with eigenvector $|u\rangle$, eigenvalue $e^{2\pi i\phi}$
- Access to controlled-$U^{2^k}$ for $k = 0, 1, \ldots, m-1$
- A preparation of $|u\rangle$

The phase estimation circuit produces:

$$
\frac{1}{2^{m/2}}\bigotimes_{k=0}^{m-1}\left(|0\rangle + e^{2\pi i 2^k \phi}|1\rangle\right)
= \frac{1}{2^{m/2}}\sum_{y=0}^{2^m-1} e^{2\pi i \phi y}|y\rangle
$$

Applying the inverse QFT yields the best $m$-bit approximation to $\phi$.

### Success probability

If $\phi$ is exactly an $m$-bit fraction ($\phi = a/2^m$), the output is deterministic. Otherwise, the best $m$-bit approximation is obtained with probability $\geq 4/\pi^2 \approx 0.405$.

**Amplification (Appendix C):** To get an estimate within $1/2^{n+1}$ of $\phi$ with probability $\geq 1 - \epsilon$, use $m = n + \lceil\log_2(1/(2\epsilon) + 1/2)\rceil$ bits. This adds $O(\log(1/\epsilon))$ controlled-power gates to the phase-estimation register; if controlled powers are synthesized by repeated calls to $U$, the underlying $U$-query/time cost scales with the largest power used.

### The $|1\rangle$ trick for order-finding

When the eigenvectors of $U$ are unknown (as in Shor's algorithm), use a computational basis state such as $|1\rangle$ instead. For modular multiplication of order $r$, this state decomposes into a uniform superposition of eigenstates, so phase estimation samples a random eigenphase $k/r$. A single sample recovers $r$ only when $\gcd(k,r)=1$; otherwise it gives a denominator dividing $r$, and Shor's algorithm uses repeated samples and classical checks, multiples, or lcm post-processing to recover the order.

**This shows Shor's order-finding routine as phase estimation** — controlled modular multiplication is the controlled-$U$, and the QFT register extracts an eigenphase. Recovering the order still requires the usual continued-fraction and candidate-validation post-processing.

---

## Improved Deutsch-Jozsa (Section 3)

The paper reviews a one-query Deutsch-Jozsa presentation and frames it through phase kickback/interference. The original [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] single-bit algorithm was probabilistic, while the exact Deutsch-Jozsa algorithm itself predates this paper; CEMM's contribution is the clean interferometric presentation and related improvements, not origination of deterministic Deutsch-Jozsa.

Also generalises to:
- $f: \{0,1\}^n \to \{0,1\}^m$ with parity promise
- Affine linear functions $f(x) = Ax \oplus b$ where the full matrix $A$ can be determined with $m$ evaluations

---

## Universal interference pattern generation (Section 7)

Given any function $\phi(x)$ computable by a reversible circuit, you can create the state $\sum_x e^{2\pi i \phi(x)}|x\rangle$ using a single eigenstate in the auxiliary register and a controlled addition circuit. The key is that adding $a$ to a register in a QFT eigenstate $\sum_y e^{-2\pi i y/2^m}|y\rangle$ kicks back the phase $e^{2\pi i a/2^m}$.

This is the conceptual ancestor of later "phase function" techniques used in quantum signal processing and block encoding.

---

## Key results

| Result | Statement |
|---|---|
| Unifying framework | Many central quantum algorithms can be viewed as phase estimation or interferometric phase readout |
| QFT circuit | Standard $O(m^2)$-gate circuit for $F_{2^m}$; QFT output is unentangled |
| Phase estimation | $m$-bit estimate of eigenphase with probability $\geq 4/\pi^2$; amplifiable to $1-\epsilon$ with $O(\log 1/\epsilon)$ overhead |
| Shor = phase estimation | Order-finding is phase estimation on $U: |x\rangle \to |ax \bmod N\rangle$; use $|1\rangle$ as surrogate eigenstate |
| Universal phase generation | Any computable phase function can be implemented via controlled addition on a QFT eigenstate |

---

## Why this matters

1. **Conceptual unification.** Before this paper, Deutsch-Jozsa, Bernstein-Vazirani, and Shor's algorithm looked like separate inventions. The phase estimation lens revealed them as the same trick applied to different unitaries.

2. **The standard QPE algorithm.** The controlled-$U^{2^k}$ + inverse QFT circuit presented here became the textbook version of phase estimation. [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] had the idea earlier via independent phase measurements, but the single-circuit version here is what everyone implements.

3. **The $4/\pi^2$ bound.** The explicit success probability and the amplification via extra bits (Appendix C) are the standard reference for QPE error analysis.

4. **Pedagogical influence.** This paper's "interferometry" framing of quantum computation shaped how the field teaches quantum algorithms. The Mach-Zehnder → controlled-$U$ → QFT pipeline is now canonical.

---

## Reusable ideas

The main reusable ideas are already captured as trick cards:
- **[[Phase Kickback from Eigenstate Ancilla]]** — the $U|u\rangle = e^{i\phi}|u\rangle$ kickback mechanism (already attributed to this paper)
- **[[Phase Kickback from Oracle Queries]]** — the $(-1)^{f(x)}$ phase oracle conversion
- **[[Iterative Phase Estimation (Kitaev)]]** — single-ancilla / semiclassical phase-estimation descendants of the QPE framework here

The QFT circuit construction is standard enough that it doesn't need a separate trick card.

---

## Limits / caveats

- **Review / unification paper.** The individual algorithms and Kitaev's eigenvalue-measurement idea are cited as prior work. The contribution is the framework, the explicit textbook-style QPE circuit presentation, and the interference-pattern construction.
- **Requires controlled-$U^{2^k}$.** Phase estimation as presented needs efficient implementation of controlled powers of $U$. For some unitaries (like [[Hamiltonian simulation]]), constructing these is the hard part.
- **Eigenstate preparation.** The "$|1\rangle$ trick" for Shor's algorithm works because $|1\rangle$ decomposes nicely into eigenstates of the modular multiplication operator. For general unitaries, preparing or having the eigenstate is non-trivial.

---

## References within this paper

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — original quantum computation model and problem
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch & Jozsa (1992)]] — the constant-vs-balanced problem
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein & Vazirani (1993)]] — hidden linear function
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring and discrete log
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation and abelian stabiliser; the conceptual predecessor
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — discussed in Appendix B as "concatenated interference"
- Coppersmith (1994) — QFT circuit idea (IBM report)
- Griffiths & Niu (1996) — semiclassical QFT implementation

---

## Cross-links

### Paper notes
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — the first quantum algorithm
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] — constant vs balanced; improved version presented here
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — hidden string; shown as phase estimation instance
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — exponential oracle separation (similar phase structure)
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — shown here to be phase estimation
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — conceptual predecessor for phase estimation
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — discussed as concatenated interferometry
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]] — applies QPE to quantum simulation
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — uses QPE as a core subroutine
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — QPE for eigenvalue filtering in walk-based search

### Trick cards
- [[Phase Kickback from Eigenstate Ancilla]] — the core mechanism; attributed partly to this paper
- [[Phase Kickback from Oracle Queries]] — the $|-\rangle$ ancilla phase oracle conversion
- [[Superposition Query for Global Properties]] — the broader framework
- [[Iterative Phase Estimation (Kitaev)]] — semiclassical / single-ancilla QPE variants
- [[Gapped Phase Estimation]] — modern SOSSA interval-classifier descendant, not ordinary textbook QPE
- [[Coset Sampling via Fourier Transform]] — related QFT-based technique
