> **Source:** Stephen P. Jordan, Noah Shutty, Mary Wootters, Adam Zalcman, Alexander Schmidhuber, Robbie King, Sergei V. Isakov, Tanuj Khattar, Ryan Babbush, *Optimization by Decoded Quantum Interferometry*, arXiv:2408.08292, Nature **646**, 831–836 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2408.08292) · [Nature](https://doi.org/10.1038/s41586-025-09527-5)
> **Tags:** #quantum-optimization #quantum-advantage #superpolynomial-speedup #QFT #coding-theory #LDPC #Reed-Solomon #max-XORSAT #max-LINSAT #combinatorial-optimization #resource-estimation

---

## The computational problem

**max-LINSAT** over a finite field $\mathbb{F}_p$. Given a matrix $B \in \mathbb{F}_p^{m \times n}$ and subsets $F_1, \ldots, F_m \subset \mathbb{F}_p$, find $\mathbf{x} \in \mathbb{F}_p^n$ satisfying as many as possible of the $m$ constraints $\sum_j B_{ij} x_j \in F_i$.

**max-XORSAT** is the special case $p = 2$, $|F_i| = 1$: find a binary string satisfying as many linear mod-2 equations as possible.

**Optimal Polynomial Intersection (OPI)** is a special case of max-LINSAT where $B$ is a Vandermonde matrix over $\mathbb{F}_p$ and the dual code $C^\perp$ is a Reed-Solomon code. The task: find a degree-$(n-1)$ polynomial over $\mathbb{F}_p$ whose evaluations hit as many prescribed target sets as possible.

## What the paper does

Introduces Decoded Quantum Interferometry (DQI), a quantum algorithm that uses the quantum Fourier transform to reduce optimization problems to classical error-correcting code decoding problems. The core idea: prepare a quantum state whose amplitudes are a polynomial function of the objective value, so that measurement is biased toward high-value solutions. The bottleneck step — uncomputing auxiliary registers — amounts to syndrome decoding, and the power of the algorithm is directly tied to how many errors the decoder can handle.

For OPI, DQI with the Berlekamp-Massey decoder achieves a superpolynomial speedup over all known classical algorithms. For sparse max-XORSAT, DQI with belief propagation decoding beats general-purpose classical heuristics (including simulated annealing) on carefully constructed instances, though a tailored classical solver can still win.

This is a genuinely new angle on quantum optimization. Unlike QAOA or quantum annealing (which exploit local landscape structure / tunnelling), DQI exploits Fourier sparsity of the objective function. The approach also cannot be dequantized by any relativizing technique, which is a strong structural argument.

## The algorithm

### DQI for max-XORSAT (simplest case)

The objective function $f(\mathbf{x}) = \sum_{i=1}^m (-1)^{v_i + \mathbf{b}_i \cdot \mathbf{x}}$ has an extremely sparse Hadamard transform — only $m$ nonzero coefficients, located at the rows $\mathbf{b}_1, \ldots, \mathbf{b}_m$ of $B$.

DQI prepares a state $|P(f)\rangle = \sum_{\mathbf{x}} P(f(\mathbf{x})) |\mathbf{x}\rangle$ where $P$ is a degree-$\ell$ polynomial chosen to bias measurement toward strings with large $f(\mathbf{x})$. The five steps:

1. **Prepare weighted Dicke state superposition:** $\sum_{k=0}^{\ell} w_k |D_{m,k}\rangle$ where $|D_{m,k}\rangle$ is the [[Dicke State Preparation via Inequality Testing|Dicke state]] of Hamming weight $k$ on $m$ qubits. Cost: $O(m^2)$ gates.

2. **Phase kick:** Apply $Z_1^{v_1} \otimes \cdots \otimes Z_m^{v_m}$ to encode $(-1)^{\mathbf{v} \cdot \mathbf{y}}$.

3. **Compute syndrome:** Evaluate $B^T \mathbf{y}$ into an ancilla register via reversible matrix multiplication.

4. **Decode and uncompute:** Infer $\mathbf{y}$ from $B^T \mathbf{y}$ using a reversible classical decoder. This is the hard step — it requires solving the syndrome decoding problem for the code $C^\perp = \{\mathbf{d} \in \mathbb{F}_2^m : B^T \mathbf{d} = \mathbf{0}\}$ with at most $\ell$ errors. Once $\mathbf{y}$ is recovered, subtract it from the first register to uncompute.

5. **Hadamard transform** on the remaining register yields $|P(f)\rangle$.

Total gate cost: $O(T + m^2)$ where $T$ is the decoder cost.

### DQI for max-LINSAT over $\mathbb{F}_p$

Generalizes the above by replacing the Hadamard transform with the quantum Fourier transform over $\mathbb{F}_p$ and using $p$-ary Dicke states. The polynomial $P$ acts on $\mathbb{F}_p$-valued objective functions. Otherwise structurally identical.

### The optimization-to-decoding reduction

This is the key conceptual contribution. DQI maps:

| Optimization side | Coding theory side |
|---|---|
| Constraint matrix $B$ | Parity check matrix $B^T$ of code $C^\perp$ |
| Polynomial degree $\ell$ | Number of correctable errors |
| Approximation quality | Decoding radius |
| Structured $B$ (Vandermonde) | Algebraic codes (Reed-Solomon) |
| Sparse $B$ | LDPC codes |

Any advance in decoding theory automatically translates to a quantum optimization result via the [[DQI Semicircle Law for Optimization-Decoding Duality|semicircle law]].

## Key results

**Theorem 4.1 (Semicircle Law).** For a max-LINSAT instance where each constraint set has size $r$, if the dual code $C^\perp$ can be decoded from $\ell$ errors, DQI achieves:

$$\frac{\langle s \rangle}{m} = \left(\sqrt{\frac{\ell}{m}\left(1 - \frac{r}{p}\right)} + \sqrt{\frac{r}{p}\left(1 - \frac{\ell}{m}\right)}\right)^2$$

when $r/p \le 1 - \ell/m$, and $\langle s \rangle / m = 1$ otherwise. In the balanced case $r/p = 1/2$, this simplifies to $\langle s \rangle / m = 1/2 + \sqrt{(\ell/m)(1 - \ell/m)}$ — the equation of a semicircle.

**Theorem 10.1** extends this to imperfect decoding: even when $2\ell + 1 \ge d^\perp$ (beyond half the code distance), a decoder that succeeds with high probability on random errors still achieves approximately the same fraction.

**OPI result.** For the balanced case $r/p \simeq 1/2$ with the Berlekamp-Massey decoder:

$$\frac{\langle s \rangle_{\text{DQI+BM}}}{p} = \frac{1}{2} + \sqrt{\frac{n}{2p}\left(1 - \frac{n}{2p}\right)}$$

The best known classical polynomial-time algorithm (Prange) achieves only $1/2 + n/(2p)$. At $n/p = 1/10$: DQI gives 0.718 vs. Prange's 0.55. At $n/p = 1/2$: DQI gives 0.933 vs. Prange's 0.75. This is a superpolynomial speedup (assuming no better classical algorithm exists for OPI).

**Resource estimate for OPI.** At $p = 521$, $n/p = 1/2$: approximately $10^8$ logical Toffoli gates and $9 \times 10^3$ logical qubits, using Qualtran for the reversible Berlekamp-Massey decoder. This is already within plausible fault-tolerant range.

**max-XORSAT benchmarks.** For a constructed instance with 31,216 variables and 50,000 constraints:

| Algorithm | Satisfaction fraction |
|---|---|
| Tailored heuristic (7 min × 1 core) | 0.880 |
| Long anneal (73 hrs × 5 cores) | 0.832 |
| DQI + belief propagation | $\ge 0.831$ |
| Prange's algorithm | 0.812 |
| Short anneal (8 sec × 1 core) | 0.764 |

DQI+BP matches a 73-hour, 5-core simulated anneal using an 8-second belief propagation decode — five orders of magnitude faster in computational steps.

## Comparison with prior work

| Approach | What it exploits | Applies to | Speedup type |
|---|---|---|---|
| QAOA | Local landscape structure | General combinatorial problems | Unknown; believed polynomial for most problems |
| Quantum annealing | Tunnelling through barriers | Ising models | Unknown; debated |
| Chen-Liu-Zhandry (2021) | QFT + quantum decoding | Short lattice vectors | Superpolynomial (proven) |
| Yamakawa-Zhandry (2022) | QFT + folded RS codes | Oracle problems | Exponential (oracle separation) |
| [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes\|Schmidhuber et al. (2024)]] | Guided sparse Hamiltonian + Kikuchi | Planted inference | Nearly quartic |
| **DQI** | **QFT + classical decoding** | **max-LINSAT, OPI, max-XORSAT** | **Superpolynomial for OPI; TBD for generic CSPs** |

DQI is distinct from all Hamiltonian-based approaches. It's the first quantum optimization algorithm that directly exploits Fourier sparsity of the objective function rather than the structure of the optimization landscape.

Versus QAOA specifically: for $D$-regular max-$k$-XORSAT at $k = 2, 3$, QAOA actually beats the upper bound on DQI with classical decoders. DQI's advantage emerges for larger $k$ or when quantum decoders are used.

## Limits / caveats

1. **No unconditional hardness proof for OPI.** The superpolynomial speedup for OPI is conditional on no classical polynomial-time algorithm matching DQI's satisfaction fraction. Related OPI problems are known to be NP-hard or as hard as discrete log, but not in the exact parameter regime where DQI works.

2. **Classical tailored solvers can win on max-XORSAT.** The max-XORSAT instances where DQI beats simulated annealing are also solvable by a tailored classical heuristic (weighted simulated annealing). The authors are honest about this: they don't claim superpolynomial advantage for max-XORSAT.

3. **QAOA dominates DQI (with classical decoders) at small $k$.** For max-2-XORSAT and max-3-XORSAT, information-theoretic bounds show DQI can't compete with QAOA when restricted to classical decoders. Advantage requires either large $k$ or quantum decoders.

4. **Belief propagation degrades for dense parity check matrices.** When DQI is applied to large-$k$ problems, the resulting decoding problem involves denser matrices where BP is less effective. Better decoders for this regime are needed.

5. **Decoder must be implemented reversibly.** The reversible circuit overhead for classical decoders hasn't been fully quantified beyond OPI+Berlekamp-Massey.

6. **Unbiased sampling is a feature, not a bug.** DQI produces samples where all solutions with the same objective value are equally likely. This is useful for approximate counting but means it can't exploit landscape structure the way local search does.

## Reusable ideas

1. [[DQI Semicircle Law for Optimization-Decoding Duality]] — The semicircle law (Theorem 4.1) that converts any decoding radius into an optimization approximation guarantee. Any improvement in decoding theory for a code class immediately gives a quantum optimization result for the dual problem class.

2. [[QFT-Based Amplitude Shaping via Polynomial Evaluation]] — Using the quantum Fourier transform to prepare states $|P(f)\rangle$ where amplitudes are polynomial functions of the objective value, biasing measurement toward near-optimal solutions. The degree of $P$ controls the bias strength.

3. [[Optimization-to-Decoding Reduction via Code Duality]] — The structural reduction from max-LINSAT instances to syndrome decoding problems, where the constraint matrix defines the parity check matrix of the dual code. Converts the coding theory literature into a catalog of quantum optimization results.

4. [[Uncomputation via Syndrome Decoding]] — Using efficient syndrome decoding as the uncomputation step in a quantum algorithm: when the auxiliary register $|\mathbf{y}\rangle$ needs to be uncomputed but can only be inferred from the syndrome $B^T \mathbf{y}$, a reversible decoder recovers $\mathbf{y}$ and subtracts it out.

## References within this paper

Key citations with connections to the vault:

- **Regev (2005), Aharonov & Ta-Shma (2003):** Originators of the idea that QFT can reduce between lattice problems and their duals. DQI extends this to finite-field codes.
- **Chen, Liu, Zhandry (2021):** Superpolynomial quantum speedup for short lattice vectors via QFT + quantum decoding. DQI's intellectual ancestor.
- **Yamakawa & Zhandry (2022):** Oracle separation for a folded Reed-Solomon version of max-LINSAT. DQI subsumes their problem in §14 and uses it as evidence against dequantization.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]]: Resource estimation for fault-tolerant QAOA and quantum simulated annealing. DQI offers a fundamentally different approach to the same class of problems.
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes|Schmidhuber, O'Donnell, Kothari, Babbush (2024)]]: Schmidhuber is a coauthor on both. That paper targets planted inference via Kikuchi+guided Hamiltonians; DQI targets constraint satisfaction via Fourier sparsity. Different mechanisms for overlapping problem classes.
- **Berlekamp & Massey (1969):** The Reed-Solomon decoder whose reversible implementation is the bottleneck primitive for DQI on OPI.
- **Barber, Dräger (1975) / Gallager (1962):** LDPC code theory underlying the belief propagation decoder used for max-XORSAT.
- Berry, Su, Babbush et al. (2024) — [[Dicke State Preparation via Inequality Testing]]: DQI's first step is Dicke state preparation; the techniques from this trick card apply directly.
- **Qualtran (Google Quantum AI):** Used for the OPI resource estimation. The Berlekamp-Massey LFSR subroutine is costed as $\sim 10^8$ Toffolis at $p = 521$.

## My assessment

This paper is important and I think it will age well. Here's why:

**The conceptual framework matters more than the specific results.** The semicircle law creates a bridge between two mature fields (coding theory and quantum algorithms) that didn't previously have a direct operational connection for optimization. Every decoder that exists or will be invented now has a quantum optimization corollary. That's a lot of latent value.

**The OPI result is real.** The gap between DQI (0.718) and Prange (0.55) at $n/p = 1/10$ is wide enough that closing it classically would require a qualitatively new algorithm. The resource estimates ($10^8$ Toffolis, $9 \times 10^3$ qubits at $p = 521$) are already within early fault-tolerant reach — comparable to where chemistry estimates were circa 2018 before years of optimization brought them down further.

**The max-XORSAT story is honest but mixed.** They beat general-purpose heuristics but not a tailored one. The five-orders-of-magnitude runtime ratio against simulated annealing is impressive as an indicator, but as they note, it shouldn't be read as a quantum-vs-classical runtime comparison (reversibility and QEC overhead are unaccounted for).

**The QAOA comparison is revealing.** QAOA beats DQI's information-theoretic ceiling at small $k$. This means these are genuinely different tools for different regimes: QAOA for low-degree, landscape-friendly problems; DQI for high-degree or algebraically structured problems. They're not competitors; they're complementary.

**Open question that matters most:** Can quantum decoders extend DQI's advantage to generic sparse constraint satisfaction, or will classical tailored solvers always catch up on unstructured instances?

## Cross-links

### Paper notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — resource estimation for fault-tolerant QAOA and quantum simulated annealing; DQI offers a different approach to the same class of problems
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — Schmidhuber coauthor on both; that paper targets planted inference via Kikuchi-guided Hamiltonians while DQI targets constraint satisfaction via Fourier sparsity; different mechanisms for overlapping problem domains
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — QAOA is the main competing approach for max-XORSAT; DQI beats QAOA's information-theoretic ceiling at large $k$ but QAOA wins at $k=2,3$ with classical decoders
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — another Babbush-group quantum advantage result; that paper uses Hamiltonian simulation for exponential speedup, DQI uses QFT + decoding for superpolynomial optimization speedup
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — parallel question about whether quantum advantage is real; DQI provides a positive answer for a concrete structured optimization problem class

### Trick cards
- [[Dicke State Preparation via Inequality Testing]]
- [[DQI Semicircle Law for Optimization-Decoding Duality]]
- [[QFT-Based Amplitude Shaping via Polynomial Evaluation]]
- [[Optimization-to-Decoding Reduction via Code Duality]]
- [[Uncomputation via Syndrome Decoding]]
