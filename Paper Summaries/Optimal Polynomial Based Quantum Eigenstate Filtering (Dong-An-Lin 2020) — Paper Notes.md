> **Source:** Yulong Dong, Xiang Meng, K. Birgitta Whaley, and Lin Lin, *Optimal polynomial based quantum eigenstate filtering with application to solving quantum linear systems*, arXiv:1910.14596, Quantum **4**, 361 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1910.14596) · [Quantum](https://doi.org/10.22331/q-2020-11-11-361)
> **Tags:** #eigenstate-filtering #QSP #minimax-polynomial #QLSA #adiabatic #quantum-Zeno #ground-state-preparation

---

## What the paper does

Develops an **eigenstate filtering** primitive based on QSP + minimax polynomials, then applies it to solve the quantum linear system problem (QLSP) via two independent routes: one using adiabatic quantum computing (AQC), the other using the quantum Zeno effect. Both achieve near-optimal $\tilde{O}(d\kappa\log(1/\epsilon))$ query complexity for $d$-sparse matrices with condition number $\kappa$, without using phase estimation or amplitude amplification.

The eigenstate filtering result is independently useful — it's the cleanest formulation of "given a block-encoding and a gapped eigenvalue, project onto the corresponding eigenspace using QSP with an optimal polynomial."

## The eigenstate filtering algorithm

### Setup

Given: Hermitian $H$ with block-encoding $U_H$ (an $(\alpha, m, 0)$-block-encoding), a target eigenvalue $\lambda_*$ with spectral gap $\Delta$ (distance from $\lambda_*$ to the rest of the spectrum), and an initial state $|\psi\rangle$ with overlap $\gamma = \|P_{\lambda_*}|\psi\rangle\| > 0$.

Goal: prepare $P_{\lambda_*}|\psi\rangle / \|P_{\lambda_*}|\psi\rangle\|$ to fidelity $1 - \epsilon$.

### The polynomial

The key ingredient is an **optimal minimax polynomial** $R^*(\lambda)$ that solves:

$$\min_{R \in \mathcal{P}_d^{\text{even}}} \max_{\lambda \in D_\delta} |R(\lambda)|, \quad \text{subject to } R(0) = 1$$

where $D_\delta = [-1, -\delta] \cup [\delta, 1]$ and $\delta = \Delta/(2\alpha)$ is the normalised half-gap. This is a Chebyshev-like problem: find the degree-$d$ even polynomial that equals 1 at the origin and is as small as possible on $D_\delta$.

The solution is given explicitly in terms of **Chebyshev polynomials of the first kind**:

$$R^*(\lambda) = \frac{T_d(\lambda/\delta)}{T_d(1/\delta)}$$

where $T_d$ is the degree-$d$ Chebyshev polynomial. The minimax error is:

$$\max_{\lambda \in D_\delta} |R^*(\lambda)| = \frac{1}{T_d(1/\delta)} \leq 2\left(\frac{1 - \sqrt{1 - \delta^2}}{\delta}\right)^d$$

This decays exponentially in $d$ with rate controlled by $\delta$. To achieve filtering error $\epsilon_R$, need degree $d = O(\delta^{-1} \log(1/\epsilon_R)) = O((\alpha/\Delta)\log(1/\epsilon_R))$.

### QSP implementation

The polynomial $R^*(\lambda)$ satisfies $|R^*(\lambda)| \leq 1$ on $[-1, 1]$ (by construction) and has definite parity (even). By the QSP theorem (Theorem 1 in the paper, originally from Low-Chuang and Gilyén et al.), there exist QSP phase factors $\Phi = (\phi_0, \ldots, \phi_d) \in \mathbb{R}^{d+1}$ such that the $(0,0)$-block of the QSP circuit equals $R^*(\cos\theta) e^{i\phi_0}$ when applied to $U_H$.

The circuit uses $d$ applications of $U_H$ (and $U_H^\dagger$), one ancilla qubit, and $d + 1$ single-qubit rotations. Total cost: $d = O((\alpha/\Delta)\log(1/\epsilon))$ queries to $U_H$.

### Post-selection

After applying the QSP circuit to $|\psi\rangle$, measure the ancilla. On outcome $|0\rangle$ (success probability $\geq \gamma^2 (1 - \epsilon_R)^2$), the system register is close to $P_{\lambda_*}|\psi\rangle$ (normalised). The success probability can be boosted with amplitude amplification if needed, at a cost of $O(1/\gamma)$ repetitions.

**Theorem 2 (Eigenstate filtering):** The target eigenstate can be prepared to error $\epsilon$ using $O((\alpha/\Delta)\log(1/\epsilon))$ queries to $U_H$ and $O(1/\gamma)$ copies of the initial state, with $m + 1$ ancilla qubits.

## Application to QLSP

The QLSP: given $d$-sparse $A \in \mathbb{C}^{N \times N}$ with $\|A\| = 1$, condition number $\kappa$, prepare $|x\rangle \propto A^{-1}|b\rangle$.

### Algorithm 1: AQC + eigenstate filtering

This builds directly on [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]]. The idea:

1. **Embed** the linear system in the adiabatic Hamiltonian $H(s) = (1 - f(s))H_0 + f(s)H_1$ where $H_0, H_1$ are constructed so the zero-energy eigenstate at $s = 0$ is $|b\rangle$ and at $s = 1$ is $|x\rangle$.

2. **Discretise** the adiabatic path into $L$ steps. At each step, apply the eigenstate filter to project onto the ground state of $H(s_\ell)$, using the output of the previous step as the initial state.

3. The spectral gap at step $\ell$ is $\Delta_\ell = \Omega(1/\kappa)$ (uniformly, by choosing the schedule carefully), so each filtering step costs $O(\kappa d \log(1/\epsilon_\ell))$ queries.

4. The number of steps $L$ and per-step precision can be chosen so the overlaps remain bounded, giving total complexity $\tilde{O}(d\kappa\log(1/\epsilon))$.

**Theorem 3 (AQC-based QLSP):** Query complexity $\tilde{O}(d\kappa\log(1/\epsilon))$ with $O(n + \log^2(\kappa/\epsilon))$ ancilla qubits. No phase estimation, no amplitude amplification needed.

### Algorithm 2: Quantum Zeno + eigenstate filtering

An alternative route using the quantum Zeno effect:

1. **Define** a continuously deformed Hamiltonian path (same $H(s)$ as above).
2. Instead of AQC, apply **frequent projective measurements** (Zeno-style) to keep the state in the ground-state subspace.
3. Replace each "measurement" with the eigenstate filter — the QSP circuit acts as an approximate projector.
4. Interleave short Hamiltonian evolution segments with filtering steps.

The Zeno approach and AQC approach have the same asymptotic complexity, but Zeno avoids the need to carefully optimise the adiabatic schedule — the frequent projections enforce ground-state tracking regardless of schedule choice.

**Theorem 4 (Zeno-based QLSP):** Same $\tilde{O}(d\kappa\log(1/\epsilon))$ query complexity.

## Comparison with prior QLSP algorithms

| Algorithm | Queries to $U_H$ | Queries to $O_B$ | Phase est.? | Amp. amp.? |
|---|---|---|---|---|
| HHL (2009) | $\tilde{O}(d^2\kappa^2/\epsilon)$ | $O(\kappa)$ | Yes | No |
| Childs-Kothari-Somma (2017) | $\tilde{O}(d\kappa\log(1/\epsilon))$ | $O(\kappa/\epsilon)$ | Yes | No |
| Gilyén et al. (2019, QSVT) | $\tilde{O}(d\kappa\log(1/\epsilon))$ | $O(\kappa/\epsilon)$ | No | Yes |
| An-Lin (2019, AQC) | $\tilde{O}(d\kappa\,\text{polylog})$ | $O(1)$ | No | No |
| **This work (AQC + filter)** | $\tilde{O}(d\kappa\log(1/\epsilon))$ | $O(\kappa)$ | **No** | **No** |
| **This work (Zeno + filter)** | $\tilde{O}(d\kappa\log(1/\epsilon))$ | $O(\kappa)$ | **No** | **No** |

The main advance over Gilyén et al.: no amplitude amplification needed (the eigenstate filter directly produces a pure state rather than a flagged block-encoding). Over CKS: fewer initial-state queries ($O(\kappa)$ vs $O(\kappa/\epsilon)$). Over An-Lin (2019): this paper makes the AQC approach rigorous with explicit complexity bounds and adds the Zeno alternative.

## Limitations

- Requires a **known lower bound on the spectral gap** $\Delta$. Without this, the polynomial degree can't be set correctly. For systems where the gap is unknown, you'd need a separate estimation step.
- The $O(1/\gamma)$ dependence on initial overlap is hidden in the $O(\kappa)$ bound for QLSP (since for the adiabatic path, $\gamma$ at each step is $\Omega(1)$ by construction). For general eigenstate filtering, small overlap $\gamma$ means many repetitions.
- The Zeno algorithm is conceptually clean but requires more ancilla qubits than the AQC version in practice — each "measurement" = eigenstate filter uses the same QSP circuit, but the interleaving with evolution segments adds overhead.
- Non-Hermitian $A$ handled via standard dilation $\hat{A} = \begin{pmatrix} 0 & A \\ A^\dagger & 0 \end{pmatrix}$, which doubles system size and condition number.
- Numerical experiments (Section 6) show the method works well for small instances but finding QSP phase factors for large polynomial degree remains a practical challenge (solved in subsequent work by the same group).

## Reusable ideas

1. [[Chebyshev Polynomial Spectral Projector|Chebyshev minimax projector]]: $R^*(\lambda) = T_d(\lambda/\delta)/T_d(1/\delta)$ is the optimal polynomial for eigenstate filtering. Degree $d = O(\delta^{-1}\log(1/\epsilon))$, error decays exponentially with $d$. This is a standard result in approximation theory (equioscillation / Chebyshev) but the application to QSP is the contribution.

2. **Eigenstate filter as approximate projector:** Replace projective measurements in quantum Zeno arguments with QSP-implemented polynomial filters. This makes Zeno-type algorithms fully coherent and rigorous.

3. **Adiabatic-to-circuit compilation via eigenstate filtering:** Discretise an adiabatic path, apply an eigenstate filter at each step to maintain ground-state tracking. The filter cost is $O(1/\text{gap})$ per step, giving a direct conversion from adiabatic runtime to circuit query complexity.

## Cross-links

### Paper notes
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — precursor AQC-based QLSP; this paper adds the eigenstate filter to make it rigorous
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — same group, same period; uses QSP for ground-state preparation with near-optimal complexity
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT framework; their QLSP algorithm uses amplitude amplification (this paper avoids it)
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — original HHL algorithm
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — CKS QLSP via LCU
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — subsequent work by Dong and Lin on eigenvalue transformation
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP framework

### Trick cards
- [[Chebyshev Polynomial Spectral Projector]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
