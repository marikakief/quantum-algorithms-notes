> **Source:** Yulong Dong, Lin Lin, and Yu Tong, *Ground-State Preparation and Energy Estimation on Early Fault-Tolerant Quantum Computers via Quantum Eigenvalue Transformation of Unitary Matrices*, arXiv:2204.05955, PRX Quantum **3**, 040305 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2204.05955) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.3.040305)
> **Tags:** #ground-state-energy #ground-state-preparation #early-fault-tolerant #QET-U #QSP #phase-estimation #control-free

---

## What the paper does

Develops **QET-U** (quantum eigenvalue transformation of unitary matrices with real polynomials) — a framework for applying polynomial functions $f(H)$ to quantum states using the **Hamiltonian evolution** $U = e^{-iH}$ as input, **one ancilla qubit**, and **no multi-qubit control operations**. This is the natural companion to [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]], which uses block encoding; QET-U replaces that resource-heavy input model with something implementable on near-term devices.

Two algorithm variants:
1. **Short query depth** (Theorem 4): $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ total queries, $\tilde{O}(\varepsilon^{-1})$ query depth, 1 ancilla, no multi-qubit control
2. **Near-optimal** (Theorem 5): $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ total queries, 3 ancillas, low-level multi-qubit control only (Toffoli)

Both outperform all prior algorithms with comparable resource constraints for ground-state energy estimation.

## The construction

### QET-U circuit (Theorem 1)

The core observation: interleaving controlled forward ($U = e^{-iH}$) and backward ($U^\dagger = e^{iH}$) evolutions with single-qubit $X$-rotations on one ancilla qubit implements an eigenvalue transformation. For an even real polynomial $F(x)$ of degree $d$ with $|F(x)| \leq 1$ on $[-1,1]$, there exist **symmetric phase factors** $\Phi_z = (\varphi_0, \varphi_1, \ldots, \varphi_1, \varphi_0)$ such that:

$$(\langle 0| \otimes I_n)\, \mathcal{U}_{\Phi_z}\, (|0\rangle \otimes I_n) = F(\cos(H/2))$$

The circuit structure (Fig. 1(c) in the paper) is almost identical to a repeated Hadamard test — just with tuned rotation angles between each $U$/$U^\dagger$ application. This is why it's so cheap: same hardware as QPE, different software (the phase factors).

The connection to [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]: QET-U is essentially QSVT applied to a **unitary** input rather than a block-encoded matrix. The qubitization step is trivial — $U$ and $U^\dagger$ naturally provide the two reflections needed. This avoids the ancilla overhead of constructing a block encoding of $H$.

### Ground-state energy estimation via fuzzy bisection

The energy estimation works by binary search. At each step, solve a **fuzzy bisection problem**: given threshold $x$ and fuzziness $h$, determine whether $\lambda_0 \leq x - h$ or $\lambda_0 \geq x + h$.

To solve this: construct a polynomial approximating the shifted sign function $\theta(x - \mu)$ using QET-U. The polynomial $F$ satisfies $F(x) \approx c$ for $x \leq \mu$ and $F(x) \approx 0$ for $x > \mu$. Applying $F(\cos(H/2))$ to $|\phi_0\rangle$ and measuring the ancilla distinguishes the two cases because:
- If $\lambda_0 \leq x - h$: success amplitude $\geq (c - \varepsilon')\gamma$
- If $\lambda_0 \geq x + h$: success amplitude $\leq \varepsilon'$

The gap between these is $\Omega(\gamma)$, detectable by Monte Carlo sampling with $O(\gamma^{-2})$ repetitions.

### Binary amplitude estimation via QET-U (Lemma 12)

This is the technical heart of the near-optimal result. Standard amplitude estimation uses QPE-style circuits requiring $O(\log \gamma^{-1})$ ancilla qubits. Instead, view the Grover iterate $R_0 R_1$ as a **time-evolution operator** $e^{-iL}$ and apply QET-U to $L$ to distinguish $A \geq \gamma_2$ from $A \leq \gamma_1$.

The polynomial $P(x)$ approximates a sign function on $[\gamma_1, \gamma_2]$ with degree $O((\gamma_2 - \gamma_1)^{-1} \log(\delta^{-1}))$. The cost is $O(\gamma^{-1} \log \vartheta^{-1})$ queries, using only **one additional ancilla qubit** — down from $O(\log \gamma^{-1})$ ancillas in standard amplitude estimation.

This is a genuinely new idea: using QET-U recursively on the walk operator from amplitude amplification. It's what allows the near-optimal algorithm to work with just 3 ancilla qubits total.

### Convex optimization for phase factors (Section IV)

Rather than analytically constructing the approximating polynomial (which is technically possible but fiddly), they solve a convex optimization problem:

$$\min_{\{c_k\}} \max\left\{\max_{x_j \in [\sigma_+, \sigma_{\max}]} |F(x_j) - c|,\; \max_{x_j \in [\sigma_{\min}, \sigma_-]} |F(x_j)|\right\}$$

subject to $|F(x_j)| \leq c$ for all grid points. This is a standard $L^\infty$ approximation problem, solvable by off-the-shelf convex solvers (CVX). The resulting polynomials achieve near-optimal degree (matching the Chebyshev equioscillation bound) both asymptotically and in the pre-asymptotic regime.

Phase factors are then found via a quasi-Newton method on a non-convex objective that has a strongly convex landscape near a specific initial guess $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$. This works up to degree $\sim 10000$ on a laptop — more than enough for early fault-tolerant applications. Implemented in [QSPPACK](https://github.com/qsppack/QSPPACK). For theoretical analysis of why this optimisation succeeds, see [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]].

### Control-free implementation (Section VI)

For spin Hamiltonians $H = \sum_j H^{(j)}$ where each group $H^{(j)}$ anti-commutes with some Pauli string $K_j$ (i.e., $K_j H^{(j)} K_j = -H^{(j)}$), the controlled time evolution can be replaced by conjugation with controlled Pauli strings. This removes the need for directly controlling $e^{-iH}$.

For the **transverse field Ising model**: $K = Y_1 \otimes Z_2 \otimes Y_3 \otimes Z_4 \otimes \cdots$ anti-commutes with both $H^{(1)} = -\sum Z_j Z_{j+1}$ and $H^{(2)} = -g\sum X_j$, giving $\ell = 1$ (single group). The controlled Pauli strings only use controlled single-qubit gates — no controlled two-qubit gates.

For the **Heisenberg model**: need $\ell = 2$ groups with different anti-commuting Pauli strings $K_1, K_2$.

For the **Fermi-Hubbard model** (via Jordan-Wigner): $\ell = 3$ groups.

## Key results

### Ground-state energy estimation

| Setting | Query complexity | Query depth | Ancillas | Multi-qubit control |
|---|---|---|---|---|
| Short depth (Thm 4) | $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ | $\tilde{O}(\varepsilon^{-1})$ | 1 | None |
| Near-optimal (Thm 5) | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | 3 | Low (Toffoli) |

### Ground-state preparation

| Setting | Query complexity | Query depth | Ancillas | Multi-qubit control |
|---|---|---|---|---|
| Short depth (Thm 6) | $\tilde{O}(\Delta^{-1}\gamma^{-2})$ | $\tilde{O}(\Delta^{-1})$ | 1 | None |
| Near-optimal (Thm 11) | $\tilde{O}(\Delta^{-1}\gamma^{-1})$ | $\tilde{O}(\Delta^{-1}\gamma^{-1})$ | 2 | Low (Toffoli) |

### Phase estimation (Corollary 10)

When the initial state is an eigenstate ($\gamma = 1$): $\tilde{O}(\varepsilon^{-1})$ queries, 1 ancilla, Heisenberg-limited. This recovers optimal QPE complexity with minimal resources.

## Comparison with prior work

| Algorithm | Query complexity | Query depth | Ancillas | MQC |
|---|---|---|---|---|
| **This work (short depth)** | $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ | $\tilde{O}(\varepsilon^{-1})$ | 1 | None |
| **This work (near-optimal)** | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | 3 | Low |
| Semi-classical QPE [Berry 2009, Higgins 2007] | $\tilde{O}(\varepsilon^{-1}\gamma^{-4})$ | $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ | 1 | None |
| QEEA [Lin-Tong 2022] | $\tilde{O}(\varepsilon^{-4}\gamma^{-4})$ | $\tilde{O}(\varepsilon^{-1})$ | 1 | None |
| High-confidence QPE [Nagaj 2009] | $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ | $\tilde{O}(\varepsilon^{-1})$ | $O(\text{polylog})$ | High |
| [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes\|LT20]] | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ | $m + O(\log \varepsilon^{-1})$ | High (BE) |
| [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes\|Lin-Tong 2022]] | $\tilde{O}(\varepsilon^{-1}\gamma^{-4})$ | $\tilde{O}(\varepsilon^{-1})$ | 1 | None |

The short-depth algorithm achieves a **quadratic improvement** over semi-classical QPE ($\gamma^{-2}$ vs $\gamma^{-4}$) and over [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]] QEEA ($\varepsilon^{-1}\gamma^{-2}$ vs $\varepsilon^{-4}\gamma^{-4}$), all with the same resource constraints (1 ancilla, no MQC).

The near-optimal algorithm matches [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|LT20]] in query complexity while reducing ancilla count from $O(m)$ to 3 and requiring only low-level multi-qubit control.

## Connection to Lin-Tong (2022)

This paper is the natural follow-up to [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]]. Lin-Tong (2022) established the spectral-measure CDF approach and Heisenberg-limited energy estimation via block encoding; this paper replaces the block encoding with the Hamiltonian evolution input model via QET-U, making the same ideas practical for early fault-tolerant devices.

The QEEA algorithm from Lin-Tong (2022, Appendix C) achieves $\tilde{O}(\varepsilon^{-1})$ query depth with 1 ancilla but at $\tilde{O}(\varepsilon^{-4}\gamma^{-4})$ total cost. The short-depth QET-U algorithm dramatically improves this to $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ at the same query depth.

## Limits / caveats

- **Controlled Hamiltonian evolution** is still needed in general. The control-free implementation only works for Hamiltonians with appropriate anti-commutation structure (spin models). For general sparse Hamiltonians, you still need controlled $e^{-iH}$.
- **$\gamma^{-2}$ vs $\gamma^{-1}$:** The short-depth algorithm's $\gamma^{-2}$ scaling is suboptimal. Getting $\gamma^{-1}$ requires amplitude amplification (Toffoli gates, deeper circuits). For small $\gamma$, this is a real cost.
- **Trotter errors compound:** When $U = e^{-iH}$ is implemented via Trotter, the total gate complexity picks up factors of $C_{\text{Trotter}}^{1/p}\varepsilon^{-1/p}\gamma^{-1/p}$ (Appendix C). This can be significant for high accuracy.
- **Noise sensitivity:** Numerical experiments show energy estimation needs depolarizing error rate $r_{\text{dplz}} \leq 10^{-4}$ — beyond NISQ capabilities. This is genuinely early fault-tolerant, not NISQ.
- **Polynomial degree requirements grow with $\Delta^{-1}$.** For small spectral gaps, the circuit depth scales as $O(\Delta^{-1} \log \gamma^{-1})$, which may still be large on near-term hardware.
- **Open question (stated in paper):** Can the near-optimal $\gamma^{-1}$ scaling be achieved without any multi-qubit controlled operations?

## Reusable ideas

1. **[[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]]** — the core technique: applying polynomial transformations of a Hamiltonian using only its time evolution, one ancilla, and tuned X-rotations. Replaces block encoding for eigenvalue transformation tasks.

2. **[[Binary Amplitude Estimation via Recursive QET-U]]** — using QET-U on the Grover walk operator to solve binary amplitude estimation with one additional ancilla qubit, replacing the $O(\log \gamma^{-1})$ ancillas of standard amplitude estimation.

3. **[[Control-Free QET-U via Anti-Commuting Pauli Strings]]** — for spin Hamiltonians with anti-commuting structure, replacing controlled time evolution with conjugation by controlled Pauli strings, eliminating the need for controlled multi-qubit gates.

## References within this paper

- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin and Tong (2020)]] — block-encoding version with near-optimal complexity; this paper's HE-model counterpart
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin and Tong (2022)]] — QEEA and Heisenberg-limited estimation; QET-U dramatically improves the total cost
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low, Wiebe (2019)]] — QSVT framework; QET-U is the unitary-input specialization
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn, Rossi, Tan, Chuang (2021)]] — QSP/QET tutorial and unified framework
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca, Tapp (2002)]] — standard amplitude amplification used in near-optimal variant
- Ge, Tura, Cirac (2019) — faster ground state preparation via LCU of time evolution (GTC19 in tables)
- Wang, Wang, Dong, Lin (2021) — QSPPACK for efficient phase factor computation
- Wan, Berta, Campbell (2021) — randomized statistical phase estimation

## Cross-links

### Paper notes
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]]

### Trick cards
- [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]]
- [[Binary Amplitude Estimation via Recursive QET-U]]
- [[Control-Free QET-U via Anti-Commuting Pauli Strings]]
- [[Approximate CDF from Hadamard Tests]] — the Hadamard test is the degree-1 special case of QET-U
- [[Importance Sampling for Fourier CDF Estimation]] — the spectral measure approach from Lin-Tong (2022) that motivates QET-U
