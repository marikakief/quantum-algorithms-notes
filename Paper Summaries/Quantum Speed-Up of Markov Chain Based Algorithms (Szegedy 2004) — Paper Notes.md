> **Source:** Mario Szegedy, *Quantum speed-up of Markov chain based algorithms*, FOCS 2004, pp. 32–41
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0401053) · [FOCS](https://doi.org/10.1109/FOCS.2004.53)
> **Tags:** #quantum-walk #markov-chain #search #spectral-gap #grover #element-distinctness

---

## What the paper does

Introduces **quantized bipartite walks** — the quantum analogue of classical Markov chains — and proves a general quadratic speedup for search on any symmetric Markov chain. The main result is the $\sqrt{\delta\epsilon}$ rule: if a classical walk-based search costs $O(1/\delta\epsilon)$ steps (where $\delta$ is the spectral gap of the chain and $\epsilon$ is the fraction of marked elements), the quantum version costs $O(1/\sqrt{\delta\epsilon})$.

This unifies and generalizes Grover search and Ambainis's element distinctness algorithm under a single framework. It is also the foundation for the walk operators that later became [[Qubitization Iterate|qubitization]] in [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]].

---

## The setup

**Classical problem:** A symmetric Markov chain $P$ on $[n]$ with spectral gap $\delta$. Some elements are marked (set $G$), with $|G| \geq \epsilon n$ or $|G| = 0$. Distinguish these cases.

**Three subroutines with costs:**

| Subroutine | Description | Cost |
|---|---|---|
| PickUniform() | Sample from stationary distribution | $\wp_0$ |
| ApplyChain($i$) | Apply one step of $P$ from state $i$ | $\wp_1$ |
| IsMarked($i$) | Check if $i \in G$ | $\wp_2$ |

**Classical cost:** $O(\wp_0 + (\wp_1 + \wp_2)/\delta\epsilon)$

**Quantum cost:** $O(\wp_0 + (\wp_1 + \wp_2)/\sqrt{\delta\epsilon})$ — quadratic speedup in both $\delta$ and $\epsilon$.

---

## Quantized bipartite walks

### Classical bipartite walk

A bipartite walk on $[n| \cup |m]$ is a pair of stochastic matrices $(c, r)$: $c$ maps $[n| \to |m]$ and $r$ maps $|m] \to [n|$. For a Markov chain $P$ on $[n]$, take $(c, r) = (P, P)$.

### Quantization

The quantum version lives in $\mathbb{C}^{[n|} \otimes \mathbb{C}^{|m]}$. Define states:

$$
\sqrt{c_i} = \sum_j \sqrt{c[i,j]}\, |i\rangle|j\rangle, \qquad \sqrt{r_j} = \sum_i \sqrt{r[j,i]}\, |i\rangle|j\rangle
$$

and projection operators:

$$
C = \sum_{i=1}^n \sqrt{c_i}\sqrt{c_i}^\dagger, \qquad R = \sum_{j=1}^m \sqrt{r_j}\sqrt{r_j}^\dagger
$$

The **two-step walk operator** is:

$$
\mu = (2R - I)(2C - I) = \mathrm{ref}_B \cdot \mathrm{ref}_A
$$

This is a product of two reflections — the same structure that appears in [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] and later in [[Qubitization Iterate|qubitization]].

---

## The Spectral Theorem (Theorem 1)

The eigenvalues and eigenvectors of $\mu = \mathrm{ref}_B \cdot \mathrm{ref}_A$ are determined by the **discriminant matrix**:

$$
M = \begin{pmatrix} 0 & \sqrt{c \circ r^T} \\ \sqrt{r \circ c^T} & 0 \end{pmatrix} = \mathrm{Gram}(\sqrt{c_1}, \ldots, \sqrt{c_n}, \sqrt{r_1}, \ldots, \sqrt{r_m}) - I_{n+m}
$$

The eigenvalues of $M$ lie in $[-1, 1]$ and are symmetric around 0. If $\lambda$ is an eigenvalue of $M$ with eigenvector $(a, b)$, then $\mu$ has eigenvalues:

$$
e^{\pm i\theta} = 2\lambda^2 - 1 \pm 2i\lambda\sqrt{1-\lambda^2}
$$

```
 Eigenvalues of M          Eigenvalues of μ
 
 -1 ─────── 0 ─────── 1    maps to the unit circle:
                            
                              eiθ
 λ ∈ [-1,1] ──────────▶  ╱     ╲
                        ·         ·   θ = arccos(2λ²-1)
                         ╲     ╱
                           e-iθ
                            
 [−1,1] folds and wraps onto the unit circle
```

**Special cases:**
- $\lambda = 1$: eigenvalue 1 (stationary state of the walk)
- $\lambda = 0$: eigenvalue $-1$
- $|\lambda| < 1$: complex eigenvalues on the unit circle

For a symmetric Markov chain $P$, the discriminant matrix is $M = \begin{pmatrix} 0 & P \\ P & 0 \end{pmatrix}$, so the eigenvalues of $M$ are $\pm$ the eigenvalues of $P$.

---

## The $\sqrt{\delta\epsilon}$ rule (Theorem 2)

### The modified walk

Define $P'$: same as $P$ on unmarked elements, but marked elements map to themselves with probability 1. The quantized walk $\nu$ of $P'$ has half-discriminant:

$$
D = \begin{pmatrix} P_1 & 0 \\ 0 & I \end{pmatrix}
$$

where $P_1$ is $P$ restricted to unmarked elements.

**Key lemma (Lemma 2):** The spectral radius of $P_1$ is at most $1 - \delta\epsilon/2$.

This means all non-trivial eigenvalues of $\nu$ have phases $|\theta| \geq \sqrt{\delta\epsilon}$.

### The algorithm

```
1. Prepare walk state u = Σ_{i,j} √(P[i,j]/n) |i⟩|j⟩
2. Apply Hadamard to control qubit → (|0⟩u + |1⟩u)/√2
3. Apply ν^K to the |1⟩ branch (K random in [1, 1000/√(δε)])
4. Apply Hadamard⁻¹ to control → |0⟩(u+ν^K u)/2 + |1⟩(u-ν^K u)/2
5. Measure: if control = |1⟩ or walk register is marked → output "marked"
```

**If $G = \emptyset$:** $\nu = \mu$, so $\nu^K u = u$, control always reads $|0\rangle$. Output 0 with certainty.

**If $|G| \geq \epsilon n$:** The phases $\theta_k \geq \sqrt{\delta\epsilon}$ cause $\nu^K u$ to decorrelate from $u$ for random $K \in [1, 1000/\sqrt{\delta\epsilon}]$, making $\|u + \nu^K u\|/2 \leq 7/8$ with probability $\geq 1/6$.

---

## Consequences

**Element distinctness (Corollary 1):** Given oracle $f: X \to Y$, distinguish "all distinct" from "collision exists" in $O(|X|^{2/3})$ queries. Set $\alpha = 2/3$ and use the Johnson graph walk.

**Grover on expanders (Theorem 4):** Grover search in $\sqrt{n}$ steps using only edges of a $d$-regular expander.

**Triangle finding** (Magniez-Santha-Szegedy): $O(n^{1.3})$ using this framework.

---

## Why this matters for quantum algorithms beyond search

The quantized walk operator $\mu = (2R-I)(2C-I)$ is a product of two reflections. This same structure appears in:

- [[Qubitization Iterate|Qubitization]] (Low & Chuang 2019): the walk operator encodes a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of the Hamiltonian, and QSP/QSVT applies polynomials to its eigenvalues
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry & Childs (2012)]]: quantum walk simulation of sparse Hamiltonians
- [[Oblivious Amplitude Amplification (Robust)|Amplitude amplification]]: Grover's algorithm is the special case with a trivial (complete-graph) Markov chain

The Spectral Theorem (Theorem 1) — relating eigenvalues of the discriminant matrix to eigenvalues of the walk operator via the map $\lambda \mapsto e^{\pm i\arccos(2\lambda^2-1)}$ — is the ancestor of the eigenvalue relationship in [[Qubitization Iterate|qubitization]].

---

## Reusable ideas

1. **[[Quantized Bipartite Walk Construction]]:** Given a bipartite walk $(c, r)$, build projections $C = \sum_i \sqrt{c_i}\sqrt{c_i}^\dagger$ and $R = \sum_j \sqrt{r_j}\sqrt{r_j}^\dagger$, then the walk operator is $\mu = (2R-I)(2C-I)$. This is the template for quantum walk algorithms.

2. **[[Discriminant Matrix Spectral Theorem]]:** The spectrum of $\mu = \mathrm{ref}_B \cdot \mathrm{ref}_A$ is fully determined by the discriminant matrix $M = \mathrm{Gram}(\{v_i\}, \{w_j\}) - I$. Eigenvalue $\lambda$ of $M$ maps to $e^{\pm i\arccos(2\lambda^2-1)}$ of $\mu$.

3. **Absorbing marked states:** Modify the walk so marked elements become absorbing ($P'[i,i] = 1$ for $i \in G$). This cleanly separates the spectrum: eigenvalues from $P_1$ (unmarked subchain) determine the detection speed, and the spectral radius bound $\rho(P_1) \leq 1 - \delta\epsilon/2$ gives the $\sqrt{\delta\epsilon}$ speedup.

4. **Interferometric detection via control qubit:** Split into $|0\rangle u + |1\rangle \nu^K u$, then interfere. If $\nu^K u \approx u$: constructive interference, control reads $|0\rangle$. If $\nu^K u$ has drifted: destructive interference, control reads $|1\rangle$ with constant probability.

---

## References within this paper

- Ambainis (2003, quant-ph/0311001) — element distinctness quantum walk; this paper generalizes its framework
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — database search; special case of the $\sqrt{\delta\epsilon}$ rule with $\delta = 1$
- Magniez, Santha & Szegedy (2003, quant-ph/0310134) — triangle finding using quantized walks
- Childs & Eisenberg (2003, quant-ph/0311038) — streamlined analysis of Ambainis's walk
- Childs, Cleve, Deotto, Farhi, Gutmann & Spielman (2003, quant-ph/0209131) — exponential speedup via continuous quantum walk
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]]
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs & Goldstone (2004)]] — continuous-time quantum walk search on lattices; complementary to Szegedy's discrete-time framework
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — survey of quantum walk algorithms; element distinctness via Johnson graph walk
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — the element distinctness algorithm that Szegedy's framework generalises
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — extends Szegedy's quantization to arbitrary Hamiltonians; connects walks to simulation

---

## Cross-links

### Paper notes
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — [[Qubitization Iterate|qubitization]] descends from this walk construction
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — quantum walk simulation of sparse Hamiltonians
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] — [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Markov chain ↔ Hamiltonian]] connection
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — [[QSVT Meta-Template|QSVT]] as the polynomial-transform generalization
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — unifies this framework with Ambainis's; uses phase estimation on $W(P)$ for approximate reflection
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]] — applies quantum walks to triangle finding; Szegedy is co-author
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — generalises the walk construction $G = UPU - P$ from discriminants to all frustration-free Hamiltonians
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]] — uses Szegedy walks for quantum sampling in partition function estimation
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]] — coinless quantum walk on implicitly-defined backtracking trees; related construction but starts from root instead of stationary distribution

### Trick cards
- [[Quantized Bipartite Walk Construction]]
- [[Discriminant Matrix Spectral Theorem]]
- [[Qubitization Iterate]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]]
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
