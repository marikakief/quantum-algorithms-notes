> **Source:** Dominic W. Berry and Andrew M. Childs, *Black-box Hamiltonian simulation and unitary implementation*, arXiv:0910.4157, Quantum Information and Computation **12**, 29–62 (2012)  
> **Links:** [arXiv](https://arxiv.org/abs/0910.4157) · [QIC](https://doi.org/10.26421/QIC12.1-2)  
> **Tags:** #hamiltonian-simulation #quantum-walk #black-box #historical

---

## What the paper does

Two main results: (1) improved black-box sparse Hamiltonian simulation using quantum walks, achieving linear scaling in both sparsity $D$ and simulation time $t$; (2) black-box unitary implementation via Hamiltonian simulation, achieving $\tilde O(N^{2/3})$ query complexity.

The prior state of the art (Berry–Ahokas–Cleve–Sanders 2006) had $D^4$ dependence on sparsity. This paper reduces that to linear in $D$ for the sparse case. That is the headline result.

## Walk operator construction

Given black-box access $O_H$ to matrix elements $H_{jk}$, construct an isometry $T: |j\rangle \mapsto |\eta_j\rangle$ where

$$
|\eta_j\rangle = |j\rangle\left(\sqrt{\frac{\varepsilon}{|H|_1}}\sum_k \sqrt{H^*_{jk}}|k\rangle + \sqrt{1 - \frac{\varepsilon\sigma_j}{|H|_1}}|M+1\rangle\right),
$$

with $\sigma_j = \sum_k |H_{jk}|$ and $\|H\|_1 = \max_j \sigma_j$ (the max column-sum norm). The walk step is $V = iS(2TT^\dagger - \mathbf{1})$ where $S$ swaps the two registers.

The key relation is $\langle\eta_j|S|\eta_k\rangle = \varepsilon H_{jk}/|H|_1$, so the walk operator's eigenphases encode the eigenvalues of $H/\|H\|_1$ scaled by $\varepsilon$. Phase estimation on $V$ recovers these phases, and thus the Hamiltonian spectrum.

The parameter $\varepsilon \in (0,1]$ controls **laziness**: small $\varepsilon$ makes the walk slow, which reduces the arcsin nonlinearity error in the phase-eigenvalue relation. The paper uses phase estimation to correct residual errors from this nonlinearity.

## Main complexity results

**Theorem 1 (sparse simulation):** With upper bounds $\Lambda \ge \|H\|$ and $\Lambda_{\max} \ge \|H\|_{\max}$, simulate evolution for time $t$ with error $\delta$ using

$$
O\!\left(\frac{\Lambda t}{\sqrt{\delta}} + D\Lambda_{\max} t + 1\right) \text{ queries to } O_H,\, O_F.
$$

Linear in both $D$ and $\Lambda t$. The $\Lambda t/\sqrt{\delta}$ term comes from phase estimation; the $D\Lambda_{\max}t$ term from implementing each walk step for the sparse case.

**Theorem 2 (non-sparse simulation):**

$$
O\!\left(D^{2/3}[(\log\log D)\,\Lambda t]^{4/3}\,\delta^{-1/3}\right) \text{ queries}.
$$

Achieved via a decomposition of $H$ into magnitude bands (Section VII). The $D^{2/3}$ scaling arises from balancing the band decomposition cost against the per-term simulation cost.

**Corollary 3 (black-box unitary):**

$$
O\!\left(N^{2/3}(\log\log N)^{4/3}\,\delta^{-1/3}\right) \text{ queries to } O_U.
$$

Much better than the $\Omega(N^2)$ elementary gates needed for an explicitly specified unitary. Matches the search lower bound $\Omega(\sqrt{N})$ only for "typical" unitaries (numerically observed to require $\tilde O(\sqrt{N})$).

## State preparation via amplitude amplification

In the non-sparse case, preparing $|\varphi_j\rangle$ exactly is expensive (the amplitudes are proportional to $\sqrt{H^*_{jk}}$, which can be arbitrary). The paper uses Grover-style [[Standard Amplitude Amplification|amplitude amplification]] to prepare these states with fewer queries, following Ref. [12] (Grover 2000) with a modification for the ancilla orthogonality condition. This reduces state-preparation cost from $O(N)$ to $O(N^{2/3})$ per walk step.

## Black-box unitary via Hamiltonian embedding

To implement an unknown unitary $U$ given oracle access to its matrix elements, embed it as

$$
H = \begin{pmatrix} 0 & U \\ U^\dagger & 0 \end{pmatrix}.
$$

Evolving under $H$ for time $t = \pi/2$ maps between the two sectors and applies $U$ (up to a global phase). This reduces unitary implementation to Hamiltonian simulation, transferring the walk-based simulation complexity directly.

## Historical context

| Approach | Sparsity scaling | Time scaling |
|---|---|---|
| Lloyd (1996) | — (local Hamiltonians only) | polynomial |
| Aharonov–Ta-Shma (2003) | $D^6$ | $\tilde O(t)$ |
| Berry–Ahokas–Cleve–Sanders (2006) | $D^4$ | near-linear in $t$ |
| **This paper, sparse (Thm 1)** | **$D$** | **linear in $t$** |
| **This paper, non-sparse (Thm 2)** | **$D^{2/3}$** | **$(\Lambda t)^{4/3}$** |

The reduction from $D^4$ to $D$ for sparse Hamiltonians is the headline improvement.

## Limits

- Uses the max column-sum norm $\|H\|_1$, not the spectral norm $\|H\|$. For non-sparse Hamiltonians these can differ by $\sqrt{N}$, which causes exactly the problems studied in Childs–Kothari (0908.4398).
- Not optimal: later qubitization/QSP/QSVT frameworks achieve better parameters and cleaner structure for oracles that provide block-encodings.
- Worst-case constant prefactors are not particularly tight.

## Reusable ideas

1. **[[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]]** — encode Hamiltonian matrix elements into walk eigenphases via the $T: |j\rangle \mapsto |\eta_j\rangle$ isometry construction. Applies whenever you have query access to matrix elements and want a walk operator whose spectrum encodes the Hamiltonian.

2. **[[Lazy-Walk Phase-Correction for Simulation Accuracy]]** — use the laziness parameter $\varepsilon$ to suppress the arcsin nonlinearity in the walk eigenphases, trading walk speed for simulation accuracy. A precursor to the Bessel linearisation of BCK 2015.

3. **[[Magnitude-Banded Hamiltonian Decomposition]]** — decompose a non-sparse Hamiltonian into magnitude bands to control the column-sum norm. Used here for the non-sparse simulation result.

4. **[[Block-Hamiltonian Embedding for Unitary Synthesis]]** — embed a target unitary into a Hamiltonian via $H_U = \begin{pmatrix} 0 & U \\ U^\dagger & 0 \end{pmatrix}$ and simulate to implement $U$. Achieves $\tilde{O}(N^{2/3})$ query complexity for $N \times N$ unitaries.

---

## References within this paper

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — prior sparse simulation with $D^4$ dependence that this paper improves to $D$
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|Childs & Kothari (2010)]] — lower bounds on non-sparse simulation
- Childs (2010, Commun. Math. Phys. 294) — quantum walk approach to sparse simulation (linear in $D$ via different walk construction)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — [[Quantized Bipartite Walk Construction|quantization of Markov chains]], foundation for quantum walk methods
- Grover (2000) — [[Standard Amplitude Amplification|amplitude amplification]] used for state preparation in the walk step
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — first sparse Hamiltonian simulation, $D^6$ scaling
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — walk-based linear-time simulation for non-sparse Hamiltonians

---

## Cross-links

- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]]
- [[Lazy-Walk Phase-Correction for Simulation Accuracy]]
- [[Amplitude-Amplified State Preparation inside Walk Operators]]
- [[Magnitude-Banded Hamiltonian Decomposition]]
- [[Block-Hamiltonian Embedding for Unitary Synthesis]]
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] — combines this walk with Bessel-LCU for near-optimal simulation in all parameters
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
