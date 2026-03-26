# SOS Decomposition via Semidefinite Programming for Chemistry

> **Source:** Low, King, Berry et al., arXiv:2502.15882
> **Tags:** #trick #sum-of-squares #SDP #quantum-chemistry #classical-preprocessing

## What it does
Computes a sum-of-squares (SOS) decomposition of a fermionic Hamiltonian at polynomial classical cost. The SOS lower bound $E_{\text{SOS}}$ determines the gap parameter for [[SOS Spectral Amplification|spectrum amplification]].

## The trick

The electronic structure Hamiltonian $H$ can be written as $H = \sum_\alpha O_\alpha^\dagger O_\alpha + E_{\text{SOS}}$ by solving the SDP:

$$\max\; E_{\text{SOS}} \quad \text{s.t.}\quad H - E_{\text{SOS}} I = \vec{o}^\dagger G \vec{o}, \quad G \succeq 0$$

where $\vec{o}$ is a vector of fermionic operators defining the SOS algebra. The Gram matrix $G$ is factored to extract the $O_\alpha$ operators.

**Choice of algebra matters.** The spin-free level-2 algebra uses generators:

$$O_\alpha = \sum_{j\sigma} c^\alpha_{j\sigma} a_{j\sigma} + \sum_{j\sigma} d^\alpha_{j\sigma} a^\dagger_{j\sigma} + \left(e^\alpha I + \sum_{ij} g^\alpha_{ij} \sum_\sigma a^\dagger_{i\sigma} a_{j\sigma}\right)$$

This is the dual of the variational 2-RDM method. The SDP constrains pseudomoment matrices to be PSD — exactly the N-representability conditions (DQG conditions in the full spinful case).

**Key insight:** Expanding the algebra (level-3, etc.) tightens $E_{\text{SOS}}$ toward $E_{\text{gs}}$, but increases both the classical cost and the block-encoding cost of the resulting SOS Hamiltonian. For chemistry, the spin-free level-2 algebra already gives $E_{\text{gap}} \sim N^{0.88}$ — small enough for substantial speedups from spectrum amplification.

## When to reach for it
- Pre-processing step for any [[SOS Spectral Amplification|SOS-based spectrum amplification]] of a fermionic Hamiltonian
- Any time you need a certified lower bound on $E_{\text{gs}}$ that also provides a constructive operator decomposition
- Connects to the 2-RDM variational method — if you already do N-representability relaxations, the dual SDP gives you the SOS decomposition for free

## Complexity
SDP with spin-free level-2 algebra: poly($N$) classical cost. The cuLoRADS GPU solver handles up to ~150 orbitals. Low-rank factorization with rank $O(\log m)$ (where $m$ is the number of linear constraints) suffices empirically.

## Caveat
The SDP finds the *optimal* $E_{\text{SOS}}$ for the chosen algebra, but this may not be the best choice for the quantum algorithm. The [[DFTHC Factorization|DFTHC]] optimization directly minimizes $\lambda_{\text{sqrt}}$ and $E_{\text{gap}}$ jointly, which can give better overall cost even with a slightly suboptimal $E_{\text{SOS}}$.

Full spinful DQG algebra gives tighter gaps but has only been computed for $\leq 60$ orbitals and produces coefficients that break spin symmetry in the block-encoding.

## Related notes
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
- [[SOS Spectral Amplification]]
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Polynomial Completion via Sum-of-Squares]]
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
