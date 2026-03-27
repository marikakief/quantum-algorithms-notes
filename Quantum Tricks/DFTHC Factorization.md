
> **Source:** Low, King, Berry et al., arXiv:2502.15882
> **Tags:** #trick #tensor-factorization #quantum-chemistry #block-encoding #SOS

## What it does
Provides a three-parameter $(R, B, C)$ factorization of the two-electron integral tensor that smoothly interpolates between [[Double Factorisation for Block-Encoding|double factorization]] and [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|tensor hypercontraction]]. The representation is natively in sum-of-squares form, making it directly compatible with [[SOS Spectral Amplification|spectrum amplification]].

## The trick

The DFTHC representation of the spin-free Hamiltonian:

$$H_{\text{DFTHC}} = \sum_{pq} h'^{(1)}_{pq} E_{pq} + \frac{1}{2} \sum_{r \in [R], c \in [C]} \left(W^{(rc)} I + \sum_{b=0}^{B-1} w^{(rc)}_b \sum_{pq} u^{(r)}_{b,p} u^{(r)}_{b,q} E_{pq}\right)^2 - \frac{1}{2}\sum_{rc} (W^{(rc)})^2$$

The squared terms are already SOS — each is $O^\dagger O$ for appropriate $O$.

**Three parameters control the circuit cost:**
- $R$ = number of ranks (rotations). DF has $R = \Theta(N)$; DFTHC uses $R = \tilde{O}(1)$
- $B$ = number of basis vectors per rank. Both DF and DFTHC use $B = \Theta(N)$
- $C$ = number of copies per rank. DF has $C = 1$; THC effectively has $C = \Theta(N)$; DFTHC uses $C = \Theta(N)$

**Why it works:** The block-encoding cost has three main components:
1. **Rot** (Givens rotation oracle): $\sim N + RB$ Toffolis — dominates in DF
2. **Qroam** (coefficient lookup): $\sim RBC/(\lambda+1) + \lambda b_Q$ Toffolis — dominates in THC
3. **Sel** (Majorana operators): $\sim 4(N-1)b_{\text{rot}}$ Toffolis

DFTHC balances Rot and Qroam costs (Amdahl's law), achieving near-linear total Toffoli scaling $\sim N^{2.09}$ per walk step.

**Optimization:** Parameters $(u, w)$ are found via Adam gradient descent (JAX, auto-differentiated) on a loss that jointly minimizes Frobenius error $\|h^{(2)} - h^{(2)'}\|_{\text{fro}}$, block-encoding normalization $\Lambda$, and SOS gap $E_{\text{gap}}$. Random unit-vector initialization suffices (no chemistry-motivated warm start needed).

## When to reach for it
- Ground-state energy estimation of molecular Hamiltonians via qubitization + phase estimation
- Any setting where you want an SOS representation of a two-body fermionic Hamiltonian with controlled circuit cost
- When neither pure DF nor pure THC is optimal for your system size (which is most of the time)

## Complexity
Block-encoding Toffoli cost: $\sim 0.079 \times N^{2.09}$ million (empirical). $\lambda_{\text{sqrt}}^2 \sim N^{1.46}$. DFTHC optimization: one gradient step costs $O(RCN^4)$ multiplications; ~$10^{5-6}$ steps needed.

## Caveat
The nonlinear optimization has many local minima. $\varepsilon_{\text{corr}}$ (correlation energy error from DFTHC approximation) fluctuates with rank and initial conditions — doesn't decrease monotonically. Multiple restarts and statistical selection are needed for reliable results. The exhaustive $(R, B, C)$ search space is large; heuristic choices (Section IV of the paper) work well but leave room for further optimization.

## Related notes
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]
- [[Double Factorisation for Block-Encoding]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Coherent Alias Sampling for PREPARE]]
- [[SOS Decomposition via Semidefinite Programming for Chemistry]]
