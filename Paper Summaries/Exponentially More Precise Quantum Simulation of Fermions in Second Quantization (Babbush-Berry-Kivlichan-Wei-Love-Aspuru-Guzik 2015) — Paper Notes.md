> **Source:** Ryan Babbush, Dominic W. Berry, Ian D. Kivlichan, Annie Y. Wei, Peter J. Love, Alán Aspuru-Guzik, *Exponentially more precise quantum simulation of fermions I: Quantum chemistry in second quantization*, New J. Phys. 18, 033032 (2016). arXiv:1506.01020
> **Links:** [arXiv](https://arxiv.org/abs/1506.01020) · [NJP](https://doi.org/10.1088/1367-2630/18/3/033032)
> **Tags:** #quantum-chemistry #hamiltonian-simulation #LCU #taylor-series #second-quantization #fermionic

---

## The computational problem

Simulate time evolution $e^{-iHt}$ of the molecular electronic-structure Hamiltonian in second quantization to precision $\varepsilon$, using $\Theta(N)$ qubits for $N$ spin-orbitals. The Hamiltonian is

$$H = \sum_{pq} h_{pq}\, a_p^\dagger a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs}\, a_p^\dagger a_q^\dagger a_r a_s,$$

where $h_{pq}$ and $h_{pqrs}$ are the standard one- and two-electron integrals over a chosen single-particle basis.

After [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner]] (or [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Bravyi-Kitaev]]) mapping, this becomes a sum of $\Gamma = O(N^4)$ Pauli-product terms — a [[Linear Combination of Unitaries (LCU)|linear combination of unitaries]].

---

## What the paper does

First application of the [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|truncated Taylor series]] simulation technique to chemistry. Two algorithms:

1. **Database algorithm:** $\widetilde{O}(N^8 t)$ gates. Classical precomputation stores all $O(N^4)$ molecular integrals; the PREPARE oracle loads them from a lookup table.
2. **On-the-fly algorithm:** $\widetilde{O}(N^5 t)$ gates. Computes molecular integrals coherently on the quantum computer using [[On-the-Fly Coherent Molecular Integral Computation|Gaussian integral arithmetic]], avoiding the classical database entirely.

Both achieve $\log(1/\varepsilon)$ precision dependence — exponentially better than Trotter-based chemistry simulation. The on-the-fly algorithm was, at the time, the lowest gate count for second-quantized quantum chemistry simulation in the literature.

---

## The algorithm

### Hamiltonian decomposition

After the [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]], the Hamiltonian is a weighted sum of $\Gamma = O(N^4)$ Pauli-product unitaries:

$$H = \sum_{\gamma=1}^{\Gamma} W_\gamma H_\gamma,$$

with $\Lambda = \sum_\gamma |W_\gamma|$ serving as the $\ell_1$ norm. In the worst case $\Lambda \in O(N^4)$ (dominated by the two-electron terms).

### Algorithm 1: Database approach

This directly instantiates the [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] truncated Taylor series framework:

1. **Segment:** Divide $t$ into $r = \lceil \Lambda t / \ln 2 \rceil$ segments (the [[Taylor Series Truncation with ln2 Segmentation|$\ln 2$ segmentation trick]]).
2. **Taylor expand** each segment to order $K = O(\log(r/\varepsilon)\log\log(r/\varepsilon))$.
3. **PREPARE:** Prepare the ancilla superposition $\frac{1}{\sqrt{s}} \sum_{k,\gamma_1,\ldots,\gamma_k} \sqrt{\beta_{k,\gamma_1,\ldots,\gamma_k}}\, |k\rangle|\gamma_1\rangle\cdots|\gamma_k\rangle$ using:
   - A cascade of controlled $R_y(\theta_k)$ rotations on a unary-encoded $|k\rangle$ register, with angles $\theta_k = 2\arcsin\!\left(\sqrt{1 - \frac{(\Lambda t/r)^{k-1}/(k-1)!}{\sum_{q=k}^{K}(\Lambda t/r)^q/q!}}\right)$. This distributes probability across Taylor orders proportional to $(\Lambda t/r)^k / k!$.
   - $K$ copies of prepare$(W)$, each preparing the coefficient state for the $\gamma$-register from the classical database. Cost: $O(\Gamma)$ gates per copy.
4. **SELECT:** $K$ sequential controlled applications of select$(H)$, which applies $H_\gamma$ conditioned on $|\gamma\rangle$. Each select$(H)$ costs $O(N)$ gates via [[Dynamic SELECT for Second-Quantized Hamiltonians|dynamic Pauli-string construction]] from the orbital indices.
5. **[[Oblivious Amplitude Amplification (Robust)|Robust OAA]]:** Since $s \approx 2$, a single round of oblivious amplitude amplification deterministically implements the segment evolution.

**Total cost:** $O(rK\Gamma) = \widetilde{O}(\Lambda t \cdot K \cdot N^4) = \widetilde{O}(N^8 t)$ gates.

### Algorithm 2: On-the-fly approach

The key idea: instead of storing all $O(N^4)$ integrals classically and loading them via a database oracle, compute them coherently on the quantum computer. This replaces the $O(\Gamma) = O(N^4)$ cost of prepare$(W)$ with $O(N)$ per query.

The construction:

1. **Rewrite the Hamiltonian** as an integral over a continuous auxiliary variable $z$:
$$H = \int W(z)\, H(z)\, dz,$$
where $W(z)$ encodes the molecular integrals as a function of the auxiliary variable (related to the Coulomb interaction via the Boys function / Gaussian integral decomposition).

2. **Discretise** the integral on $M$ quadrature points, giving $H \approx \sum_{m=1}^{M} W(z_m) H(z_m) \Delta z$.

3. **PREPARE now has two parts:**
   - Prepare the quadrature-point register: a superposition over $m$ with amplitudes proportional to $\sqrt{|W(z_m)|}$. This is the [[On-the-Fly Coherent Molecular Integral Computation|on-the-fly integral computation]] — evaluating the Gaussian basis functions at each quadrature point using $O(\log N)$ ancilla arithmetic.
   - Prepare the orbital-index register conditioned on $z_m$: because the integrand factorises (each two-electron integral decomposes into products of one-electron integrals when expressed via the auxiliary variable), this costs only $O(N)$ instead of $O(N^2)$.

4. **SELECT** is the same as Algorithm 1: $O(NK)$ gates.

**Total cost:** $\widetilde{O}(N \cdot K \cdot \lambda t) = \widetilde{O}(N^5 t)$ gates, where $\lambda$ is the discretised integral normalisation (bounded by $\widetilde{O}(N^4)$ but the per-query cost drops from $O(N^4)$ to $O(N)$).

---

## Key results

| Quantity | Database algorithm | On-the-fly algorithm |
|---|---|---|
| Gate count | $\widetilde{O}(N^8 t)$ | $\widetilde{O}(N^5 t)$ |
| Precision dependence | $O(\log(1/\varepsilon) \log\log(1/\varepsilon))$ | Same |
| Qubits (system) | $\Theta(N)$ | $\Theta(N)$ |
| Ancilla qubits | $\Theta(K \log \Gamma)$ | $\Theta(K \log(M\mu))$ |
| PREPARE cost per call | $O(\Gamma) = O(N^4)$ | $O(N)$ |
| SELECT cost per call | $O(NK)$ | $O(NK)$ |

The $\widetilde{O}$ suppresses $\text{polylog}(N, 1/\varepsilon)$ factors from the Taylor truncation order $K$.

---

## Comparison with prior work

| Method | Gate count | Precision scaling | Year |
|---|---|---|---|
| Trotter-Suzuki (2nd order) | $O(N^{10} t^2 / \varepsilon)$ | $O(1/\varepsilon)$ | Pre-2015 |
| [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes\|Babbush et al. (2015) Trotter analysis]] | Tighter empirically but still $\text{poly}(1/\varepsilon)$ | $\text{poly}(1/\varepsilon)$ | 2015 |
| This paper (database) | $\widetilde{O}(N^8 t)$ | $\log(1/\varepsilon)$ | 2015 |
| This paper (on-the-fly) | $\widetilde{O}(N^5 t)$ | $\log(1/\varepsilon)$ | 2015 |

The jump from $\text{poly}(1/\varepsilon)$ to $\log(1/\varepsilon)$ is the paper's main contribution to chemistry simulation. The $N$-dependence is less competitive — later work ([[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|sublinear block-encoding methods]], double factorisation, tensor hypercontraction) substantially reduces the $N$-scaling.

---

## Limits / caveats

- The $N^8$ (database) and $N^5$ (on-the-fly) scaling in $N$ is quite steep. Modern methods (sparse qubitisation, double factorisation, THC) achieve much better $N$-dependence.
- The $\widetilde{O}$ hides significant polylog factors. The practical gate count for realistic molecules can be large.
- The on-the-fly algorithm requires evaluating Gaussian integrals on a quantum computer to exponential precision, which involves non-trivial quantum arithmetic circuits (additions, multiplications, function evaluations). The paper argues the cost is $O(\text{polylog})$ per integral but doesn't give explicit circuit constructions.
- The $\Lambda \in O(N^4)$ upper bound is pessimistic for many molecular systems — tighter bounds depend on the specific molecule and basis set. This is the same observation exploited by the companion [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Trotter error paper]].
- Part I of a two-paper series; Part II (arXiv:1506.01029) handles the configuration interaction representation.

---

## Reusable ideas

1. [[On-the-Fly Coherent Molecular Integral Computation]] — Computing molecular integrals coherently on the quantum computer (via Gaussian quadrature over an auxiliary variable) instead of classically precomputing and loading them. Reduces PREPARE cost from $O(N^4)$ to $O(N)$.

2. [[Dynamic SELECT for Second-Quantized Hamiltonians]] — Constructing the Pauli string to apply from the orbital-index register dynamically at runtime, rather than using a lookup table over all $\Gamma$ terms. Exploits the structure of [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner]]-mapped fermionic operators.

---

## References within this paper

Key papers cited:

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — The truncated Taylor series framework that both algorithms instantiate (cited as [33]).
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — The STOC 2014 precursor with fractional queries and [[Oblivious Amplitude Amplification (Robust)|OAA]] (cited as [32]).
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — Companion paper analysing Trotter errors for chemistry; same first author.
- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]] — Early fermionic simulation framework.
- Wecker, Bauer, Clark, Hastings, Troyer, *Progress towards practical quantum advantage in quantum chemistry*, PRA 92, 062318 (2015) — Practical resource estimates for chemistry simulation.
- Helgaker, Jørgensen, Olsen, *Molecular Electronic-Structure Theory* (2000) — Standard reference for one- and two-electron integrals, Boys function, Gaussian basis sets.
- Babbush, Berry, Kivlichan, Wei, Love, Aspuru-Guzik, *Exponentially more precise quantum simulation of fermions II: Configuration interaction representation*, arXiv:1506.01029 — Part II companion paper.

---

## Cross-links

### Paper notes
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — The framework this paper instantiates
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — Companion Trotter analysis by overlapping author team
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — Prior optimal-precision result (STOC 2014)
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] — Later work on fermionic state preparation
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]] — Modern block-encoding approach improving $N$-scaling
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]] — Early experimental chemistry simulation
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] — Foundational fermionic simulation

### Trick cards
- [[Linear Combination of Unitaries (LCU)]] — Core technique
- [[Taylor Series Truncation with ln2 Segmentation]] — Segmentation strategy
- [[Oblivious Amplitude Amplification (Robust)]] — Deterministic amplification
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]] — Fermion-to-qubit mapping
- [[On-the-Fly Coherent Molecular Integral Computation]] — Novel trick from this paper
- [[Dynamic SELECT for Second-Quantized Hamiltonians]] — Novel trick from this paper
