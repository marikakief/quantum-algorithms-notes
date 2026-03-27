> **Source:** Alexander Schmidhuber, Ryan O'Donnell, Robin Kothari, Ryan Babbush, *Quartic quantum speedups for planted inference*, arXiv:2406.19378, Phys. Rev. X **15**, 021077 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2406.19378) · [Phys. Rev. X](https://doi.org/10.1103/PhysRevX.15.021077)
> **Tags:** #quantum-advantage #super-quadratic-speedup #planted-inference #Kikuchi-method #guided-Hamiltonian #amplitude-amplification #phase-estimation #cryptography #LPN #Tensor-PCA

---

## The computational problem

**Planted Noisy $k$XOR** (a.k.a. Sparse Learning Parity with Noise). Given $m$ equations of the form $x_{S_i} = b_i$ where each scope $S_i \subset [n]$ has $|S_i| = k$ and $x_S := \prod_{i \in S} x_i$, decide whether the instance is drawn from:

- **Random:** scopes uniform random, right-hand sides i.i.d. Rademacher ($\pm 1$ with equal probability), or
- **Planted:** a secret $z \in \{±1\}^n$ is chosen, scopes are uniform random, and $b_i = \eta_i z_{S_i}$ where $\mathbb{E}[\eta_i] = \rho$ (the "planted advantage"; $\rho = 1 - 2\eta$ for noise rate $\eta$).

The constraint density $\Delta = m/n$ controls the signal-to-noise ratio. The classical computational threshold for degree-$\ell$ Sum-of-Squares (and the Kikuchi method) is:

$$\Delta \geq C_\kappa \cdot (n/\ell)^{(k-2)/2}$$

At this threshold, the Kikuchi method runs in $\widetilde{O}(n^\ell)$ time and $\widetilde{O}(n^\ell)$ space.

---

## What the paper does

Shows that any classical Kikuchi-method algorithm running in $\widetilde{O}(n^\ell)$ time can be "quantized" to a quantum algorithm running in $n^{\ell/4} \cdot \text{poly}(n)$ time using only $\widetilde{O}(\log n)$ qubits — a nearly quartic speedup with exponentially less space.

This is not a speedup for one specific parameter setting. It's a *meta-result*: whenever a classical "Alice Theorem" proves the Kikuchi method works at some $\ell$, a quantum "Bob" can beat Alice by a factor of $\approx n^{3\ell/4}$. The speedup holds even against hypothetical improved classical algorithms within the Kikuchi framework.

The approach also gives a quartic speedup for Tensor PCA (recovering and simplifying Hastings's result), and is expected to generalize to other planted inference problems solvable via the Kikuchi method.

**My assessment:** This is one of the most significant results in practical quantum speedups since Babbush et al.'s "Focus Beyond Quadratic Speedups" (2021) argued that quartic speedups are the sweet spot for fault-tolerant quantum advantage. This paper delivers exactly that — a quartic speedup for a natural, cryptographically relevant problem. The resource estimates ($10^{15}$–$10^{16}$ Toffoli gates for classically intractable instances) are comparable to early FeMoCo estimates, and those came down by $>10^6 \times$ with optimization. The approach is also compositional: it upgrades *any* Kikuchi-style classical algorithm, so classical improvements to the method automatically translate to quantum improvements.

---

## The algorithm

### High-level structure

1. **Preprocessing (classical):** Choose Kikuchi parameter $\ell$ and eigenvalue threshold $\lambda_{th}$ from the classical "Alice Theorem." Partition the input instance $\hat{\mathcal{I}}$ randomly into $\mathcal{I}$ (fraction $1 - \zeta$, used for the Kikuchi Hamiltonian) and $\mathcal{I}_{\text{guide}}$ (fraction $\zeta \approx 1/\ln n$, used for the guiding state).

2. **Guiding state preparation:** From $\mathcal{I}_{\text{guide}}$, construct the sparse state $|\psi\rangle = \frac{1}{\sqrt{m}} \sum_i b_i |S_i\rangle$, then form $|\Psi\rangle$ by taking $\ell/k$ copies of $|\psi\rangle$ and symmetrizing into the $\binom{n}{\ell}$-dimensional Kikuchi space. This costs $O(\ell m \log n)$ gates.

3. **Amplitude amplification loop** (repeat $\widetilde{O}(n^{\ell/4})$ times):
   - Prepare $|\Psi\rangle$
   - Run [[Qubitization (Quantum Walk for Spectral Encoding)|quantum phase estimation]] on the Kikuchi Hamiltonian $\mathcal{K}_\ell$
   - Measure the eigenvalue register; check if any eigenvalue exceeds $\lambda_{th}$

4. **Output:** If any round finds an eigenvalue above threshold → "Planted." Otherwise → "Random."

### The Kikuchi Hamiltonian

The Kikuchi method maps a degree-$k$ polynomial optimization problem to a degree-2 problem by introducing $\binom{n}{\ell}$ new variables $y_T$ indexed by $\ell$-subsets $T \subset [n]$. Each original $k$XOR constraint $x_S = b$ generates $\binom{n-k}{\ell - k/2}\binom{k}{k/2}$ new equations $y_T y_U = b$ for pairs $(T, U)$ with $T \triangle U = S$. The resulting edge-signed graph — the Kikuchi graph $\mathcal{K}_\ell$ — is a sparse Hamiltonian of dimension $\binom{n}{\ell}$, representable on $\approx \ell \log n$ qubits.

In the planted case, the "lifted" assignment $z^{\odot \ell}$ (whose $T$-coordinate is $z_T = \prod_{i \in T} z_i$) certifies that $\lambda_{\max}(\mathcal{K}_\ell) \geq \rho' \cdot d$ where $d$ is the average degree. In the random case, matrix Chernoff bounds give $\lambda_{\max}(\mathcal{K}_\ell) \leq \kappa \cdot d$ for $\kappa < \rho$.

### Where the quartic speedup comes from

Two independent quadratic factors multiply:

| Source | Classical cost | Quantum cost | Speedup |
|---|---|---|---|
| **Amplitude amplification** | $\widetilde{O}(n^\ell)$ (iterate over $n^\ell$-dim. space) | $\widetilde{O}(n^{\ell/2})$ (Grover-like) | $n^{\ell/2}$ |
| **Guiding state overlap** | Random vector: overlap $\gamma^2 = \Theta(1/n^\ell)$ | Constructed $|\Psi\rangle$: overlap $\gamma^2 = \widetilde{\Theta}(1/n^{\ell/2})$ | $n^{\ell/4}$ |
| **Combined** | $\widetilde{O}(n^\ell)$ | $n^{\ell/4} \cdot \text{poly}(n)$ | $\approx n^{3\ell/4}$ |

The guiding state overlap improvement is the technically interesting part. Classically, even if you have a good guiding vector, the Power Method still costs $\Omega(n^\ell)$ per iteration (you must multiply an $n^\ell$-dimensional vector by an $n^\ell \times n^\ell$ matrix). Quantumly, [[Qubitization (Quantum Walk for Spectral Encoding)|phase estimation]] on the sparse Hamiltonian costs only $\text{poly}(n)$ per query, and [[Kaiser Window Amplitude Estimation|amplitude amplification]] converts the overlap advantage into an additional quadratic runtime reduction.

### Why classical algorithms can't exploit the guiding state

The classical bottleneck is not the number of iterations — it's the per-iteration cost. Even with a perfect guiding vector, the Power Method on an $N \times N$ matrix costs $\Omega(N)$ per step (matrix-vector multiplication). The quantum algorithm avoids this because sparse [[Hamiltonian simulation]] queries individual matrix entries rather than multiplying the full matrix.

This connects to a deeper complexity-theoretic point: the Guided Local Hamiltonian problem with $1/\text{poly}(N)$ overlap is BQP-complete (Gharibian & Le Gall 2022), while with $2^{-N}$ overlap it's QMA-complete. The transition from exponential to polynomial overlap changes the quantum complexity dramatically (exponential → polynomial), but classical complexity stays exponential throughout. Planted inference problems naturally land in the intermediate regime — polynomial but not constant overlap — which is where quantum advantage lives.

---

## Key results

**Theorem (Main, informal).** If a classical Kikuchi-method algorithm solves Planted Noisy $k$XOR at constraint density $\Delta$ in time $\widetilde{O}(n^\ell)$, then there exists a quantum algorithm for the same problem at the same $\Delta$ with gate complexity $n^{\ell/4} \cdot \widetilde{O}(n^{k/2})$ and $\widetilde{O}(\log n)$ qubits.

**Concrete example ($k = 4$, $\rho = 0.25$).** Classical: $\widetilde{O}(n^{32})$ time and space at $\Delta \sim 0.25 n \ln n$. Quantum: $\widetilde{O}(n^{10})$ time, $\widetilde{O}(\log n)$ qubits. Speedup ratio: $n^{32}/n^{10} \approx n^{22}$, which approaches quartic as $\ell$ grows (here $32/10 \approx 3.2$).

**Tensor PCA.** Same framework gives a quartic speedup for detecting a rank-one spike in a noisy $k$-tensor. This recovers and simplifies Hastings (2020), which required Gaussian anticoncentration arguments (Carbery-Wright theorem) and multiple noisy guiding states.

**Overlap theorem (Theorem 42).** Given a planted $k$XOR instance, the guiding state $|\Psi\rangle$ constructed from the constraint right-hand sides has overlap with the cutoff eigenspace of $\mathcal{K}_\ell$ at least:

$$\langle v | \Gamma \rangle^2 \geq \xi \cdot \left(\frac{\hat{m}}{\binom{n}{k}}\right)^{\ell/k}$$

where $\xi = \text{Part}_k(\ell) \cdot \frac{\rho \epsilon \nu}{200 \ell \ln n} \cdot (\rho^2 \zeta)^{\ell/k}$. For the natural parameter setting $m = \widetilde{\Theta}(n^{k/2})$, this gives overlap $\widetilde{\Omega}(n^{-\ell/2})$.

**Resource estimates.** Preliminary Qualtran implementations suggest $\sim 10^{15}$–$10^{16}$ Toffoli gates for classically intractable instances. Block-encoding the Kikuchi matrix is cheap ($\sim 10^6$ Toffoli gates); the dominant cost is repeated phase estimation + amplitude amplification. Comparable to early FeMoCo estimates.

---

## Comparison with prior work

| Approach | Problem | Speedup | Qubits | Generality |
|---|---|---|---|---|
| Hastings (2020) | Tensor PCA only | $\sim n^{3\ell/4}$ | $O(\ell \log n)$ | Gaussian spikes, intricate proof |
| **This paper** | kXOR + Tensor PCA + planted inference | $\sim n^{3\ell/4}$ | $\widetilde{O}(\log n)$ | Any Kikuchi-solvable problem; simpler proof |
| Grover-based | Any search problem | $n^{\ell/2}$ | $O(\ell \log n)$ | Random guiding state only |
| Sum-of-Squares (classical) | kXOR | — | $n^\ell$ classical space | $n^{O(\ell)}$ time |
| Dalzell et al. (2023) | Exact optimization | super-Grover | — | Short-path algorithm; different regime |

Key differences from Hastings: (1) works for $k$XOR, not just Tensor PCA; (2) simpler proof via second-moment method instead of Carbery-Wright anticoncentration; (3) independent batch splitting replaces Hastings's multiple noisy guiding states; (4) framework is modular — any "Alice Theorem" yields a quantum algorithm.

---

## Limits / caveats

- **Not a problem-specific algorithm.** The quartic speedup is relative to the Kikuchi method specifically. If someone finds a fundamentally different classical algorithm for Planted Noisy $k$XOR that doesn't go through the Kikuchi matrix, this result says nothing about it.
- **Constant-factor gap.** For small $\ell$, the speedup exponent $\ell/(\ell/4 + k/2)$ is less than 4. The quartic ratio is asymptotic in $\ell \to \infty$. For the worked example ($k=4$, $\ell=32$), the ratio is $\approx 3.2$.
- **Resource requirements.** $10^{15}$–$10^{16}$ Toffoli gates is still large. The authors note these are from a "coarse first pass" and expect significant optimization is possible (cf. FeMoCo's $>10^6 \times$ reduction).
- **Planted Clique is excluded.** It's already a degree-2 problem, so the Kikuchi lifting doesn't help. This framework doesn't apply to all planted inference problems — only those where $k > 2$ and the Kikuchi method is the best classical approach.
- **Cryptographic implications are suggestive, not concrete.** The paper doesn't break a specific cryptosystem, but suggests that constructions based on Sparse LPN / $k$XOR hardness may need larger security parameters against quantum attackers. The 6-ary TSA predicate (where 4 variables form a 4XOR constraint) is flagged as potentially vulnerable.

---

## Reusable ideas

1. [[Kikuchi Method for Degree Reduction]] — Transform a degree-$k$ polynomial optimization problem into a degree-2 problem by introducing $\binom{n}{\ell}$ new "lifted" variables and mapping constraints to matchings on the Kikuchi graph. The resulting sparse Hamiltonian is amenable to spectral methods (classically) and [[Hamiltonian simulation]] (quantumly).

2. [[Input-Derived Guiding State for Planted Problems]] — When a planted inference problem gives you constraint right-hand sides correlated with the planted solution, the normalized constraint vector $|\psi\rangle = \frac{1}{\sqrt{m}} \sum_i b_i |S_i\rangle$ is automatically a useful guiding state. Taking $\ell/k$ copies and symmetrizing gives overlap $\widetilde{\Theta}(n^{-\ell/2})$ with the Kikuchi eigenspace — a quadratic improvement over a random vector.

3. [[Independent Batch Splitting for Hamiltonian-Guiding State Decoupling]] — Split the input randomly into two independent batches: one to build the Hamiltonian, one to build the guiding state. This decouples the two objects (which are otherwise correlated through the shared random input), enabling a clean second-moment proof of overlap. The signal loss from splitting is only polynomial ($\zeta^{\ell}$), absorbed into the overall complexity.

4. [[Guided Sparse Hamiltonian Framework]] — When a planted inference problem maps to a sparse Hamiltonian with an efficiently preparable guiding state having $\gamma^2 = 1/\text{poly}(N)$ overlap with the cutoff eigenspace, combine sparse [[Hamiltonian simulation]] + phase estimation + amplitude amplification to decide in $\widetilde{O}(s/(\gamma \alpha \lambda))$ queries. This is a general framework — not specific to $k$XOR.

---

## References within this paper

Key citations that locate this work intellectually:

- **Hastings (2020)** — Quantum 4, 237. The direct predecessor: quartic speedup for Tensor PCA via a different (more intricate) proof strategy. This paper generalizes and simplifies Hastings's approach.
- **Wein, Alaoui, Moore (2019)** — FOCS 2019. Introduced the Kikuchi method (independently of Hastings) for planted inference. The classical algorithm that this paper "quantizes."
- **Babbush, McClean, Newman, Gidney, Boixo, Neven (2021)** — PRX Quantum 2, 010103. "Focus Beyond Quadratic Speedups" — argued that quartic speedups are the practically relevant regime for fault-tolerant quantum computing. This paper is essentially the realization of that vision for a concrete problem class.
- **Gharibian & Le Gall (2022)** — STOC 2022. Guided Local Hamiltonian is BQP-complete with $1/\text{poly}$ overlap. Provides the complexity-theoretic foundation for why guiding states enable quantum advantage.
- **Raghavendra, Rao, Schramm (2017)** — STOC 2017. SoS-based algorithm for Planted Noisy $k$XOR; the Kikuchi method improves on this.
- **Dalzell, Pancotti, Campbell, Brandão (2023)** — STOC 2023. "Mind the gap" — another super-Grover speedup using guiding states + phase estimation. Similar conceptual toolkit, different problem domain.
- Berry, Childs, Kothari (2015) — FOCS 2015. Optimal sparse [[Hamiltonian simulation]] used as a subroutine.
- Gilyen, Su, Low, Wiebe (2019) — STOC 2019. [[Qubitization (Quantum Walk for Spectral Encoding)|QSVT]] framework; provides the block-encoding and [[Hamiltonian simulation]] primitives.

---

## Cross-links

### Paper notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Same author group (Kothari, Babbush); also uses sparse Hamiltonian structure for exponential (not quartic) speedup; different problem domain (coupled oscillators vs. planted inference)
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — Kothari and Babbush; different problem but same Google Quantum AI group
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Introduced qubitization for chemistry; phase estimation subroutine used here
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]] — Schmidhuber is a coauthor on both; DQI targets constraint satisfaction via Fourier sparsity while this paper targets planted inference via Kikuchi+guided Hamiltonians; different mechanisms for overlapping problem classes (both address forms of noisy $k$XOR)
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al 2024]]

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — Phase estimation on the Kikuchi Hamiltonian uses qubitization-style spectral encoding
- [[Kikuchi Method for Degree Reduction]] — Core classical technique that this paper quantizes
- [[Input-Derived Guiding State for Planted Problems]] — The guiding state construction
- [[Independent Batch Splitting for Hamiltonian-Guiding State Decoupling]] — The decoupling trick
- [[Guided Sparse Hamiltonian Framework]] — The general quantum algorithmic framework
