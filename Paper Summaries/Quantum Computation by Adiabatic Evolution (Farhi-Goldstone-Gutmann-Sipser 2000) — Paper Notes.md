> **Source:** Edward Farhi, Jeffrey Goldstone, Sam Gutmann, and Michael Sipser, *Quantum Computation by Adiabatic Evolution*, arXiv:quant-ph/0001106 (2000)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0001106)
> **Tags:** #adiabatic #satisfiability #spectral-gap #hamiltonian-simulation #foundational

---

## What the paper does

Proposes **adiabatic quantum computation** as a new model for solving combinatorial optimization problems. The idea: encode the solution to a SAT problem as the ground state of a "problem Hamiltonian" $H_P$, start in the easily-prepared ground state of a "beginning Hamiltonian" $H_B$, and slowly interpolate $H(t) = (1-t/T)H_B + (t/T)H_P$. By the adiabatic theorem, if the spectral gap stays open, the system tracks the ground state and arrives at the solution.

This paper launched the entire field of adiabatic quantum computation. It was later shown by [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] that adiabatic QC is polynomially equivalent to circuit-model QC.

---

## The algorithm

### Problem Hamiltonian $H_P$

For a SAT instance with clauses $C_1, \ldots, C_M$ on $n$ bits, define energy functions $h_C(z) = 1$ if assignment $z$ violates clause $C$, else 0. Then:

$$
H_P = \sum_C H_{P,C}, \qquad H_{P,C}|z\rangle = h_C(z)|z\rangle
$$

$H_P$ is diagonal in the computational basis, non-negative, and $H_P|z\rangle = 0$ iff $z$ satisfies all clauses. Finding the ground state = solving SAT.

### Beginning Hamiltonian $H_B$

$$
H_B = \sum_C H_{B,C}, \qquad H_{B,C} = H_B^{(i_C)} + H_B^{(j_C)} + H_B^{(k_C)}, \qquad H_B^{(i)} = \tfrac{1}{2}(1 - \sigma_x^{(i)})
$$

Ground state: $|x_1 = 0\rangle \cdots |x_n = 0\rangle = \frac{1}{2^{n/2}} \sum_{z} |z\rangle$ — the uniform superposition, easy to prepare.

Alternatively: $H_B = \sum_{i=1}^n d_i H_B^{(i)}$ where $d_i$ is the number of clauses involving bit $i$.

### Interpolation

$$
\tilde{H}(s) = (1-s)H_B + s H_P, \qquad s = t/T
$$

By the adiabatic theorem, if $g_{\min} = \min_{s} (E_1(s) - E_0(s)) > 0$, then for $T \gg \mathcal{E}/g_{\min}^2$ (where $\mathcal{E} = \max_s |\langle 1;s | d\tilde{H}/ds | 0;s \rangle|$), the system ends in the ground state of $H_P$.

Since $d\tilde{H}/ds = H_P - H_B$ and both have eigenvalues $O(\mathrm{poly}(n))$, the key quantity is $g_{\min}$. Polynomial $T$ requires $g_{\min} \geq 1/\mathrm{poly}(n)$.

---

## When does the gap stay open?

### Level crossing and symmetry

The paper gives a clear argument for why level crossings are generically avoided:
- A $2 \times 2$ Hamiltonian $\begin{pmatrix} a(s) & c(s)+id(s) \\ c(s)-id(s) & b(s) \end{pmatrix}$ has degenerate eigenvalues only when $a = b$, $c = 0$, $d = 0$ simultaneously — three conditions for one parameter $s$, so generically no crossing.
- **Symmetry** reduces the conditions. If $[\tilde{H}(s), V] = 0$ for some unitary $V$, the Hilbert space decomposes into symmetry sectors. Levels in different sectors can cross freely; levels within the same sector still generically avoid crossing.

**Implication:** The relevant gap is the smallest energy difference between the two lowest states *in the same symmetry sector as the initial state*.

### Four examples

| Problem | $g_{\min}$ | Running time $T$ | Notes |
|---|---|---|---|
| 2-SAT on a ring (agree/disagree) | $\sim 4\pi/(3n)$ | $O(n^3)$ | Solved via Jordan-Wigner + Fourier; fermion diagonalization |
| Grover problem (single $n$-bit clause) | $\sim 2 \cdot 2^{-n/2}$ | $O(2^n)$ | No speedup over classical; matches $\Omega(2^{n/2})$ query lower bound |
| Bush of implications | $\sim n^{-3/8}$ | $O(n^{1.75})$ | Exponentially many states at energy 1, but gap is poly! |
| Overconstrained 2-SAT | $\sim n^{-0.7}$ | $O(n^{3.4})$ | $\binom{n}{2}$ clauses, highly redundant |

---

## The ring example in detail

For $n$ agree clauses on a ring:

$$
\tilde{H}(s) = (1-s)\sum_{j=1}^n (1 - \sigma_x^{(j)}) + s \sum_{j=1}^n \tfrac{1}{2}(1 - \sigma_z^{(j)}\sigma_z^{(j+1)})
$$

The paper uses a **Jordan-Wigner transformation** to fermion operators $b_j, b_j^\dagger$ satisfying $\{b_j, b_k^\dagger\} = \delta_{jk}$, then Fourier-transforms to momentum modes $\beta_p$. The Hamiltonian becomes a sum of commuting $2 \times 2$ blocks, each diagonalizable:

$$
A_p(s) = \begin{pmatrix} s + s\cos(\pi p/n) & is\sin(\pi p/n) \\ -is\sin(\pi p/n) & 4 - 3s - s\cos(\pi p/n) \end{pmatrix}
$$

with eigenvalues $E_p^\pm(s) = 2 - s \pm \sqrt{(2-3s)^2 + 4s(1-s)(1-\cos\pi p/n)}$. The minimum gap is at $s \approx 2/3$ with $g_{\min} \approx 4\pi/(3n)$.

---

## The Grover analysis

For $H_P = 1 - |w\rangle\langle w|$ (single unknown marked state), the problem reduces to $(n+1)$ dimensions by exploiting permutation symmetry. The eigenvalue equation becomes:

$$
\frac{1-s}{s} = \sum_{r=0}^{n} \frac{P_r}{r - \lambda}, \qquad P_r = \frac{1}{2^n}\binom{n}{r}
$$

At the critical $s = s^*$ where the two lowest eigenvalues are closest, $\lambda \approx \pm 2^{-n/2}\sqrt{\sum P_r/r^2}$, giving:

$$
g_{\min} \simeq 2 \cdot 2^{-n/2}
$$

Exponentially small — so adiabatic evolution gives no speedup for unstructured search. This is consistent with the $\Omega(\sqrt{N})$ query lower bound.

---

## Circuit model equivalence (Section 5)

The paper shows adiabatic evolution can be recast as a sequence of few-qubit unitaries:
1. Discretize $[0, T]$ into $M = T/\Delta$ steps
2. At each step, approximate $e^{-i\Delta H(\ell\Delta)} \approx (e^{-i\Delta u H_B/K} e^{-i\Delta v H_P/K})^K$ via [[Order-Condition Cancellation in Product Formulas|Trotter formula]]
3. $e^{-i\Delta u H_B/K}$ factors into $n$ single-qubit gates (since $H_B$ is a sum of commuting 1-local terms)
4. $e^{-i\Delta v H_P/K}$ factors into one gate per clause (commuting few-qubit terms)

Total gates: $O(T^2 \cdot \mathrm{poly}(n))$. Polynomial if $T$ is polynomial.

---

## Bush of implications: the key insight

The bush has $2^n + n$ states at energy 1 (exponentially many), yet $g_{\min} \sim n^{-3/8}$. This disproves the intuition that "many low-lying states → small gap." The distinction: the bush's low-energy states form a very different structure from the Grover problem's.

The paper also shows sensitivity to the choice of $H_B$: using $H_B' = \sum_i H_B^{(i)}$ (uniform weighting) instead of $H_B = \sum_i d_i H_B^{(i)}$ (clause-weighted) makes the gap exponentially small. This suggests the clause-weighted $H_B$ from equation (2.22) is the right default choice.

---

## Reusable ideas

1. **Adiabatic interpolation:** $H(s) = (1-s)H_B + sH_P$. Encode the answer in the ground state of $H_P$, start from an easy-to-prepare ground state of $H_B$, slowly interpolate. Running time $\sim 1/g_{\min}^2$.

2. **Clause-weighted beginning Hamiltonian:** Weight each bit's $H_B^{(i)}$ by the number of clauses it appears in ($d_i$). This can make a qualitative difference to the gap (bush of implications example).

3. **Symmetry sector restriction:** When $\tilde{H}(s)$ has a symmetry, the initial state's symmetry sector is invariant. The relevant gap is within that sector, which may be larger than the full gap.

4. **Jordan-Wigner + Fourier diagonalization:** For translation-invariant spin chains, transform to fermions, Fourier-transform to momentum modes, get commuting $2 \times 2$ blocks. Exact gap calculation.

---

## References within this paper

- Messiah (1958) — adiabatic theorem reference
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — unstructured search; the paper analyzes adiabatic Grover as a special case
- Farhi & Gutmann (1998) — continuous-time Hamiltonian version of Grover; $\Omega(\sqrt{N})$ lower bound
- Bennett, Bernstein, Brassard & Vazirani (1997) — query complexity lower bounds
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — Trotter-based simulation used in Section 5 to show circuit equivalence

---

## Cross-links

### Paper notes
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — proves this model is poly-equivalent to circuits
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] — builds on this model for state generation
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — simulates the $e^{iAt}$ that adiabatic evolution requires
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — modern analysis of the [[Order-Condition Cancellation in Product Formulas|Trotter formulas]] used in Section 5
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — QAOA: the Trotterised, parametrised version of adiabatic evolution for combinatorial optimisation
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]] — analyses AQC power: matches Grover via local scheduling, proves exponential lower bound for local-minimum traps

### Trick cards
- [[Order-Condition Cancellation in Product Formulas]]
- [[History-State Encoding with Unary Clock]] — later used to prove adiabatic ↔ circuit equivalence
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]] — later technique for bounding the gap
