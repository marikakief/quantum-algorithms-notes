> **Source:** William J. Huggins, Kianna Wan, Jarrod McClean, Thomas E. O'Brien, Nathan Wiebe, Ryan Babbush, *Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values*, arXiv:2111.09283, Physical Review Letters **129**, 240501 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.09283) · [Journal](https://doi.org/10.1103/PhysRevLett.129.240501)
> **Tags:** #measurement #expectation-value #gradient-estimation #quantum-algorithms #RDM #lower-bound #fault-tolerant

---

## The computational problem

Given a state $|\psi\rangle$ prepared by a unitary oracle $U_\psi$, and $M$ Hermitian operators $\{O_j\}$ with $\|O_j\| \leq 1$, estimate each $\langle \psi | O_j | \psi \rangle$ to within additive error $\varepsilon$ with probability $\geq 2/3$. The cost measure is the number of queries to $U_\psi$ and $U_\psi^\dagger$.

This is the multi-observable generalisation of the standard expectation value estimation problem. It arises naturally in quantum chemistry (measuring RDMs, dipole moments, forces), condensed matter (correlation functions), and anywhere a quantum simulation needs to extract multiple properties from a single prepared state.

## What the paper does

The paper gives an algorithm achieving $\tilde{O}(\sqrt{M}/\varepsilon)$ queries to the state preparation oracle, and proves this is worst-case optimal (up to logs) in the high-precision regime $\varepsilon < 1/(3\sqrt{M})$. The key idea: encode all $M$ expectation values as the gradient of a single scalar function, then apply Gilyen, Arunachalam, and Wiebe's quantum gradient estimation algorithm. This is a clean win over both amplitude estimation ($\tilde{O}(M/\varepsilon)$ queries) and sampling ($\tilde{O}(G/\varepsilon^2)$ queries, where $G$ is the number of commuting groups), in the appropriate regime.

My assessment: This is a genuinely nice result. The construction is elegant — the gradient encoding trick is simple once you see it, but the connection to the quantum gradient algorithm is non-obvious. The lower bound settling the optimality question is satisfying. For practical quantum chemistry, the main question is whether the ancilla overhead and controlled time evolution costs make it competitive with sampling-based approaches for realistic problem sizes. That question remains open.

## The algorithm / construction

### Step 1: Encode expectation values as a gradient

Define the parameterised unitary:

$$U(x) := \prod_{j=1}^{M} e^{-2ix_j O_j}$$

and the function:

$$f(x) := -\frac{1}{2} \operatorname{Im}\langle \psi | U(x) | \psi \rangle + \frac{1}{2}$$

Then $f : \mathbb{R}^M \to [0,1]$ and:

$$\frac{\partial f}{\partial x_\ell}\bigg|_{x=0} = \langle \psi | O_\ell | \psi \rangle$$

So the gradient $\nabla f(0)$ gives exactly the $M$ expectation values.

### Step 2: Build a probability oracle via Hadamard test

Construct a circuit $F(x)$ that performs the [[Analytical Gradient Estimation for Variational Quantum Circuits|Hadamard test]] for the imaginary part of $\langle \psi | U(x) | \psi \rangle$:

$$F(x) := (H \otimes I) \cdot \text{c-}U(x) \cdot (S^\dagger H \otimes U_\psi)$$

Applied to $|0\rangle \otimes |0\rangle$, this gives:

$$F(x)|0\rangle|0\rangle = \sqrt{f(x)}\,|1\rangle|\phi_1(x)\rangle + \sqrt{1-f(x)}\,|0\rangle|\phi_0(x)\rangle$$

This is a probability oracle for $f$ in the sense of Gilyen et al. Each call uses exactly one query to $U_\psi$.

### Step 3: Add quantum controls for the gradient algorithm

Build a controlled probability oracle:

$$U_f := \sum_{k \in G_n^M} |k\rangle\langle k| \otimes F(k \cdot x_{\max})$$

where $G_n^M$ is a grid of $2^{nM}$ points in an $M$-dimensional hypercube, $n = O(\log(1/\varepsilon))$, and $x_{\max} = O(1/\sqrt{M})$. The controlled time evolutions $e^{-ix O_j}$ are implemented by $n$ gates per observable, each controlled on one bit of the $j$-th index register with exponentially spaced evolution times.

### Step 4: Apply the quantum gradient algorithm

Gilyen et al.'s gradient algorithm (Theorem 25 of their paper) uses:
1. Prepare a uniform superposition over the grid $G_n^M$
2. Apply the phase oracle (converted from the probability oracle) $O(S)$ times using higher-order finite-difference formulas
3. Inverse QFT on each index register
4. Measure to extract approximate gradient components
5. Repeat $O(\log(M/\delta))$ times and take the median

### Derivative bounds

The function $f$ satisfies the analyticity condition needed by the gradient algorithm. For any $k$-th order partial derivative:

$$|\partial_\alpha f(0)| \leq 2^{k-1}$$

since $V(x,\alpha)$ in the derivative expression is a product of unitaries and bounded-norm observables. Setting $c = 2$ in Theorem 4 of Gilyen et al. gives the $\tilde{O}(\sqrt{M}/\varepsilon)$ query complexity.

## Key results

**Theorem 1 (Main algorithm).** For $M$ observables with $\|O_j\| \leq 1$ and state $|\psi\rangle$ prepared by $U_\psi$:
- Query complexity: $\tilde{O}(\sqrt{M}/\varepsilon)$ to $U_\psi$ and $U_\psi^\dagger$
- Each query uses $\tilde{O}(\sqrt{M}/\varepsilon)$ gates of the form controlled-$e^{-ixO_j}$ for each $j$, with $|x| \in O(1/\sqrt{M})$
- Space: $O(M \log(1/\varepsilon) + N)$ qubits

**Corollary 3 (Lower bound).** For $\varepsilon \in (0, 1/(3\sqrt{M}))$, any algorithm estimating $M$ expectation values to additive error $\varepsilon$ with success probability $\geq 2/3$ requires $\Omega(\sqrt{M}/\varepsilon)$ queries to $U_\psi$ in the worst case. This holds even for mutually commuting observables.

**Theorem 6 (Non-uniform norms).** For observables with $\|O_j\| \leq B_j$:
$$\text{Query complexity} = \tilde{O}\!\left(\frac{\sqrt{\sum_j B_j^2}}{\varepsilon}\right)$$

This generalisation replaces $B_{\max}\sqrt{M}$ with $\|\vec{B}\|_2$, which can be much smaller when norms are heterogeneous.

## Comparison with prior work

| Method | Query complexity | Regime | Notes |
|---|---|---|---|
| Sampling (commuting groups) | $O(G \log M / \varepsilon^2)$ | All $\varepsilon$ | $G$ = number of commuting groups; works with copies of $\|\psi\rangle$, no oracle needed |
| [[Pauli Expectation Value Estimation\|Amplitude estimation]] | $\tilde{O}(M/\varepsilon)$ | All $\varepsilon$ | Each observable estimated separately |
| Shadow tomography | $O(\log M / \varepsilon^4)$ | All $\varepsilon$ | Works with copies; bad $\varepsilon$ scaling |
| **This paper (gradient)** | $\tilde{O}(\sqrt{M}/\varepsilon)$ | $\varepsilon < 1/(3\sqrt{M})$ | Requires oracle access + controlled time evolution |

The gradient approach is optimal in the high-precision regime. For commuting observables with $G \ll M$, sampling can beat it when $\varepsilon > 1/\sqrt{M}$ (since $G\log M / \varepsilon^2 < \sqrt{M}/\varepsilon$ there). The paper discusses hybrid strategies (Appendix G) that interpolate between the two.

### Application: $k$-body RDMs

For the fermionic $k$-RDM of an $N$-mode system, there are $O(N^{2k})$ matrix elements, divided into $O(N^k)$ commuting groups. The gradient approach gives $\tilde{O}(N^k / \varepsilon)$ queries — improving over both sampling ($\tilde{O}(N^k / \varepsilon^2)$) and amplitude estimation ($\tilde{O}(N^{2k} / \varepsilon)$). The advantage over all prior methods holds when $\varepsilon = o(N^{-k/3})$.

### Application: Dynamic correlation functions

For $M$ two-point dynamic correlation functions $C_{A,B}(t) = \langle \psi | U(0,t) A^\dagger U(t,0) B | \psi \rangle$ at times $t_1 \leq \cdots \leq t_M$:
- State preparation queries: $\tilde{O}(\sqrt{M}/\varepsilon)$ (vs $\tilde{O}(M/\varepsilon)$ for amplitude estimation)
- Time evolution: $\tilde{O}(\sqrt{M}\, t_M / \varepsilon)$ (vs $\tilde{O}(\sum_j t_j \log M / \varepsilon)$)

The advantage in total time evolution depends on the time-point distribution. For evenly spaced points at interval $\Delta$: $\tilde{O}(M^{3/2}\Delta/\varepsilon)$ vs $\tilde{O}(M^2 \Delta^2 / \varepsilon)$.

## Limits / caveats

1. **High-precision regime only.** The $\sqrt{M}$ advantage requires $\varepsilon < 1/(3\sqrt{M})$. Outside this regime, sampling with commuting groups can match or beat the gradient approach.

2. **Ancilla overhead.** The algorithm needs $O(M \log(1/\varepsilon))$ additional qubits beyond the $N$-qubit system register. For $M \gg N$, this dominates the space cost. A space-query tradeoff (Appendix F) lets you partition into $g$ groups, using $\tilde{O}(N + M/g)$ qubits and $\tilde{O}(\sqrt{gM}/\varepsilon)$ queries.

3. **Controlled time evolution.** Each observable requires controlled-$e^{-ixO_j}$ gates. Total time evolution across all observables scales as $\tilde{O}(M/\varepsilon)$, same as amplitude estimation. The saving is only in state preparation queries, not in total circuit depth involving the observables.

4. **No practical resource estimates.** The paper analyses asymptotic scaling only. For actual fault-tolerant chemistry calculations, it remains unclear whether the constant factors and logarithmic overheads make this competitive with [[Basis Rotation Grouping for VQE Measurement|sampling-based approaches]] at practical system sizes.

5. **Commutativity doesn't help (in this regime).** The lower bound holds even for commuting observables, which is somewhat surprising. But it only applies in the high-precision regime — outside that regime, commutativity does help via grouping.

## Reusable ideas

1. [[Gradient Encoding for Multi-Observable Estimation]] — Encode $M$ expectation values as the gradient of a scalar function via parameterised time evolution + Hadamard test, then extract all values simultaneously using quantum gradient estimation.

2. [[Probability Oracle from Hadamard Test]] — Convert a parameterised overlap $\langle \psi | U(x) | \psi \rangle$ into a probability oracle $U_f$ with $f(x) = \frac{1}{2}(1 - \operatorname{Im}\langle \psi | U(x) | \psi \rangle)$, using the standard Hadamard test circuit (one ancilla, one $U_\psi$ query).

3. [[Non-Uniform Gradient Estimation via Variable-Precision QFT]] — Generalise Gilyen et al.'s gradient algorithm to handle gradient components with different magnitudes by allocating different numbers of qubits $n_i$ to each index register. Replaces worst-case $B_{\max}\sqrt{M}$ scaling with $\ell_2$-norm $\|\vec{B}\|_2$.

## References within this paper

Key papers cited:
- Gilyen, Arunachalam, Wiebe (2019) — Quantum gradient estimation algorithm. The primary tool; this paper's main contribution is finding the right function to feed it. No vault note.
- Jordan (2005) — Original quantum speedup for gradient estimation via binary oracle. Gilyen et al. improved it to handle probability oracles. No vault note.
- Van Apeldoorn (2021) — Lower bound for quantum probability oracles and multidimensional amplitude estimation. Provides the $\Omega(\sqrt{M}/\varepsilon)$ lower bound that Corollary 3 derives from. No vault note.
- Knill, Ortiz, Somma (2007) — Optimal single-observable estimation via amplitude estimation. The baseline $\tilde{O}(M/\varepsilon)$ when applied to each observable separately. No vault note.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins et al. (2021)]] — [[Basis Rotation Grouping for VQE Measurement|Basis rotation grouping]] for efficient VQE measurement. This paper cites it as a practical grouping strategy for the sampling-based regime.
- Huang, Kueng, Preskill (2020, 2021) — Classical shadows and information-theoretic bounds on quantum advantage in ML. The shadow tomography approaches that achieve $O(\log M)$ scaling but with $\varepsilon^{-4}$ dependence. [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Huang, Babbush, McClean et al. (2021)]] is also related.
- Zhao, Rubin, Miyake (2021) — Fermionic partial tomography via classical shadows. Gives the $\tilde{O}(N^k/\varepsilon^2)$ sampling cost for $k$-RDMs that the gradient approach improves upon.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — Cited for the dirty ancilla / QROM qubit overhead that could be reused for this algorithm's index registers.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — The [[Analytical Gradient Estimation for Variational Quantum Circuits|Hadamard test for gradient estimation]] is related but different: Romero et al. estimate gradients of the energy w.r.t. variational parameters (a classical optimisation primitive), while this paper estimates gradients of a purpose-built function to extract expectation values.

## Cross-links

### Paper notes
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — Addresses the same multi-observable estimation problem but in the shadow tomography (many copies, sampling) regime rather than the oracle/gradient regime. Achieves sample-efficient two-copy protocols for fermionic and all-Pauli sets via a graph-coloring framework.
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — Directly applies the gradient-based algorithm from this paper to molecular force estimation; develops the force-specific block encodings, importance sampling, and state recycling that make it practical for chemistry
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Same first author; that paper tackles the same measurement problem in the NISQ/sampling regime
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — Uses [[Shadow Tomography for QMC Overlap Estimation|shadow tomography]] for a different measurement problem (overlaps for QMC)
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — Shadow tomography connection
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — Measurement cost reduction for RDMs via different approach (LP constraints)
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — Dirty ancilla reuse idea
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — Hadamard test for variational gradients
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — shares lead authors (Huggins, Wan); operates in NISQ/$O(\log M / \varepsilon^2)$ regime vs. this paper's fault-tolerant $\tilde{O}(\sqrt{M}/\varepsilon)$ regime; the two approaches are complementary depending on precision requirements and hardware generation
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — uses the gradient measurement algorithm from this paper for observable estimation in first-quantized dynamics; cost $\widetilde{O}(\sqrt{L}\,\mathcal{C}_\text{samp}\,\lambda/\epsilon)$ for $L$ time points

### Trick cards
- [[Gradient Encoding for Multi-Observable Estimation]]
- [[Probability Oracle from Hadamard Test]]
- [[Non-Uniform Gradient Estimation via Variable-Precision QFT]]
- [[Pauli Expectation Value Estimation]] — The baseline approach this paper improves on
- [[Basis Rotation Grouping for VQE Measurement]] — Sampling-based alternative; better when $\varepsilon$ is not too small
- [[Shadow Tomography for QMC Overlap Estimation]] — Different approach to multi-property estimation
- [[Analytical Gradient Estimation for Variational Quantum Circuits]] — Related Hadamard test technique for variational gradients
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Uses the gradient measurement algorithm from this paper for observable estimation in first-quantized dynamics; cost $\widetilde{O}(\sqrt{L}\,\mathcal{C}_\text{samp}\,\lambda/\epsilon)$ for $L$ time points
