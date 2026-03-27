> **Source:** Dominic W. Berry, Danial Motlagh, Giacomo Pantaleoni, and Nathan Wiebe, *Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing*, arXiv:2401.10321, Phys. Rev. A **110**, 012612 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2401.10321) · [PRA](https://doi.org/10.1103/PhysRevA.110.012612)
> **Tags:** #hamiltonian-simulation #QSP #GQSP #qubitization #block-encoding #constant-factor

---

## The computational problem

Same problem as [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|standard QSP simulation]]: given a [[Block-Encoding Composition Algebra|block-encoding]] of $H/\lambda$, implement $e^{-iHt}$ on a target system to accuracy $\epsilon$.

The access model is a quantum walk operator $U$ constructed from the block-encoding as

$$U = i(2|0\rangle\langle 0| \otimes \mathbb{1}_s - \mathbb{1})V,$$

where $V$ is a self-inverse block-encoding unitary satisfying $(\langle 0| \otimes \mathbb{1})V(|0\rangle \otimes \mathbb{1}) = H/\lambda$. The eigenvalues of $U$ are $\pm e^{\pm i\arcsin(E_k/\lambda)}$.

The specific question: can we cut the query count by a factor of 2 if we can control between $U$ and $U^\dagger$ at roughly the same cost as controlling $U$?

---

## What the paper does

It halves the number of block-encoding queries for [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP-based Hamiltonian simulation]] by replacing standard controlled-$U$ operations with directionally controlled operations (selecting between $U$ and $U^\dagger$). The technique uses [[Generalized Quantum Signal Processing (GQSP)|generalized quantum signal processing]] (GQSP) from Motlagh–Wiebe (arXiv:2308.01501) to handle the resulting complex Laurent polynomials.

This is a clean factor-of-2 improvement on the dominant cost in QSP simulation. Not a new asymptotic regime — the scaling remains $O(\lambda t + \log(1/\epsilon))$ — but the leading constant drops by half, which matters in practice since these are already the tightest known bounds.

**My assessment:** This is a satisfying result. The idea is natural once you know GQSP exists: the phase-doubling trick from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] already gives $2\times$ for eigenvalue estimation via controlling $U$ vs $U^\dagger$. Extending that to simulation requires more work because you need to handle the Jacobi–Anger expansion with the wrong parity, but GQSP provides exactly the right tool for that. The paper is short (9 pages, no figures), which is appropriate — the core idea is genuinely simple once the GQSP machinery is available.

---

## The algorithm / construction

### Background: why standard QSP uses $N/2$ as the effective order

In [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low–Chuang QSP]], $N$ controlled-$U$ operations are performed, alternating with Z-rotations on an ancilla qubit. The controlled operation selects between $\mathbb{1}$ and $U$, giving effective eigenvalues $1$ and $e^{i\theta}$. By alternating which state triggers $U$, the effective rotation per step has eigenvalues $e^{\pm i\theta/2}$. So $N$ controlled operations yield Fourier components up to order $N/2$ — the order is half the query count.

### The doubling idea

Replace "control between $\mathbb{1}$ and $U$" with "control between $U$ and $U^\dagger$". Now the effective rotation per step has eigenvalues $e^{\pm i\theta}$ instead of $e^{\pm i\theta/2}$, so $N$ controlled operations give Fourier components up to order $N$. The degree-to-query ratio doubles.

### The parity problem

With directional control ($U$ vs $U^\dagger$), the accessible Fourier components of the Q-polynomial (corresponding to $\sin[\tau\sin\theta]$ in the [[Jacobi-Anger Truncation for QSP Simulation|Jacobi–Anger expansion]]) have only **even** orders. But the Jacobi–Anger expansion of $\sin[\tau\sin\theta]$ needs **odd** orders:

$$\sin[\tau\sin\theta] = 2\sum_{k\text{ odd}>0} J_k(\tau)\sin(k\theta).$$

### The GQSP fix

Multiply $C(\theta)$ by $e^{i\theta}$ to shift even components to odd. This makes $C$ complex-valued, which standard QSP (with its real-polynomial constraints) cannot handle. But [[Generalized Quantum Signal Processing (GQSP)|GQSP]] (Theorem 1, from Motlagh–Wiebe arXiv:2308.01501) allows complex polynomial pairs $(P, Q)$ with the only constraint being $|P(z)|^2 + |Q(z)|^2 = 1$ on the unit circle.

The paper reformulates GQSP for directionally controlled unitaries:

**Theorem 2 (GQSP with directional control).** For $d \in \mathbb{N}$, there exist rotation parameters such that

$$\left(\prod_{j=1}^{d} R(\theta_j, \phi_j) \begin{pmatrix} U & 0 \\ 0 & U^\dagger \end{pmatrix}\right) R(\theta_0, \phi_0, \lambda) = \begin{pmatrix} P(U) & -Q(U)^\dagger \\ Q(U) & P(U)^\dagger \end{pmatrix}$$

if and only if (1) $P, Q \in \mathbb{C}[z^{-1}, z]$ with $\deg \leq d$, (2) parity of $P, Q$ equals $d \bmod 2$, and (3) $|P(z)|^2 + |Q(z)|^2 = 1$ on $|z| = 1$.

The block structure $\begin{pmatrix} P & -Q^\dagger \\ Q & P^\dagger \end{pmatrix}$ is proved by induction in Appendix B.

### Constructing $P$ and $Q$

Set $K$ even with $K \geq \tau := \lambda t$. The target polynomials are:

$$P_K(U) = J_0(\tau) + \sum_{m=1}^{K/2} J_{2m}(\tau)(U^{2m} + U^{-2m})$$

$$Q_K(U) = i\sum_{m=0}^{K/2} J_{2m+1}(\tau)(U^{2m+1} - U^{-(2m+1)})$$

These have only odd powers of $U$ (after accounting for the Chebyshev/sin expansion structure), in steps of 2 — matching the directional-control constraint.

To satisfy $|P|^2 + |Q|^2 = 1$ exactly (GQSP requires it, not just approximately), use the [[Polynomial Completion via Sum-of-Squares|completion procedure]]:

1. Scale both by $\alpha < 1$ so that $|\alpha P_K|^2 + |\alpha Q_K|^2 \leq 1$ everywhere.
2. Find correction polynomials $P', Q'$ with $P'^2 + Q'^2 = 1 - \alpha^2(P_K^2 - Q_K^2)$.
3. Set $P(U) = U(\alpha P_K(U) + iP'(U))$ and $Q(U) = \alpha Q_K(U) + Q'(U)$.

The correction step requires $1 - \alpha^2(P_K^2 - Q_K^2) \geq 0$ for all $x$ (not just $|x| \leq 1$), which is Theorem 3.

### Parity management in the completion

The polynomial $1 - \alpha^2(P_K^2 - Q_K^2)$ is even in $x = \cos\theta$, so its roots come in $\pm$ pairs. Factor as in Rudin's sum-of-squares decomposition. The factor pairs maintain the parity structure: real parts even, imaginary parts odd. So the resulting $P'$ (even) and $Q'$ (odd) have the correct parities to combine with $P_K$ and $Q_K$ in the GQSP framework.

### Removing the extra $U$ factor

The GQSP construction produces $UP$ in the top-left block. Two additional controlled operations — one initial controlled-$U$, one final controlled-$U^\dagger$ — strip this factor:

$$\begin{pmatrix} U^\dagger & 0 \\ 0 & \mathbb{1} \end{pmatrix} \begin{pmatrix} P & -Q^\dagger \\ Q & P^\dagger \end{pmatrix} \begin{pmatrix} \mathbb{1} & 0 \\ 0 & U \end{pmatrix} = \begin{pmatrix} U^\dagger P & -Q^\dagger \\ Q & P^\dagger U \end{pmatrix}.$$

Projecting onto $|+\rangle$ on the ancilla qubit recovers $\cos[\tau\sin(H/\lambda)] - i\sin[\tau\sin(H/\lambda)] = e^{-iHt}$.

---

## Key results

**Main result:** [[Hamiltonian simulation]] via GQSP with directionally controlled walk steps uses

$$\frac{K+1}{2} + 1 \approx \frac{K}{2}$$

applications of $U$-vs-$U^\dagger$ control, where $K$ is the Jacobi–Anger truncation order. This is half the $K+1$ controlled-$U$ operations needed in [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|standard QSP simulation]].

The two additional controlled operations at the boundary (Eq. 11) are a negligible additive contribution.

**Theorem 3 (positivity):** For $K \geq 2$ even and $0 \leq \tau \leq K$,

$$P_K(x)^2 - Q_K(x)^2 \leq 1 \quad \text{for } |x| \geq 1.$$

This ensures the completion polynomial $1 - \alpha^2(P_K^2 - Q_K^2)$ is non-negative everywhere, making the sum-of-squares decomposition valid.

---

## Comparison with prior work

| Method | Queries to block-encoding | Requires complex polynomials? | Key constraint |
|---|---|---|---|
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes\|Standard QSP]] (Low–Chuang 2017) | $O(\lambda t + \log(1/\epsilon))$ | No (real $A, C$) | Control $\mathbb{1}$ vs $U$ |
| **This paper (GQSP doubling)** | $O(\lambda t/2 + \log(1/\epsilon)/2)$ | Yes (complex $P, Q$) | Control $U$ vs $U^\dagger$ |
| [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes\|Fully-coherent one-shot QSP]] (Martyn et al. 2023) | $O(\alpha t + \log(1/\epsilon) + \log(1/\delta))$ | Real (via [[Affine Spectrum Compression for QSP Parity Bypass\|affine compression]]) | Fully coherent, no postselection |
| [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes\|Stochastic QSP]] (Martyn–Rall 2025) | Halves $\log(1/\epsilon)$ cost | N/A (randomized channel) | Output is mixed state |

The $2\times$ improvement here and the $2\times$ improvement of stochastic QSP target different parts of the complexity: this paper halves the entire query count (both the $\lambda t$ and $\log(1/\epsilon)$ terms), while stochastic QSP halves only the precision-dependent term. In principle they could compose, but this paper's approach requires coherent directional control, while stochastic QSP produces a channel.

The factor-of-2 improvement from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] for eigenvalue estimation was the direct precursor. That result used the same $U$-vs-$U^\dagger$ control trick but only for phase estimation, where the polynomial is simpler. Extending to simulation requires handling the full Jacobi–Anger expansion, which is what GQSP enables.

---

## Limits / caveats

1. **The directional-control assumption.** The $2\times$ speedup requires controlling between $U$ and $U^\dagger$ at cost comparable to a single controlled-$U$. For qubitization-based block-encodings (the standard case in chemistry and materials), this is typically true — you flip $U$ to $U^\dagger$ by moving a reflection before or after $V$, which only requires controlling the reflection. But there are block-encoding constructions where this isn't cheap. The paper's claim of "very broad" applicability is fair for the current dominant paradigm, but not universal.

2. **Constant factor, not asymptotic.** The improvement is a constant $2\times$ on the query count. No change to asymptotic scaling. For near-term resource estimates this matters a lot (halving T-gate count for fault-tolerant chemistry, say), but for complexity theory it's invisible.

3. **Classical preprocessing.** The GQSP phase-finding problem (computing the rotation angles $\vec{\theta}, \vec{\phi}$) is at least as hard as the standard QSP phase-finding problem, potentially harder due to the complex polynomial constraints. The paper doesn't address this, but it's a practical consideration — [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|existing QSP phase-finding algorithms]] would need to be adapted for the GQSP setting.

4. **The completion step.** Constructing the correction polynomials $P', Q'$ requires root-finding for a degree-$4K$ polynomial (via Rudin's decomposition). This is a classical preprocessing cost, analogous to the [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials|complementary polynomial construction]] in standard QSP. Numerically, this is fine for moderate $K$ but could be delicate for very large truncation orders.

---

## Reusable ideas

1. [[Directional Walk Control for Phase Doubling]] — replacing control-$\mathbb{1}$-vs-$U$ with control-$U$-vs-$U^\dagger$ to double the effective phase per step. This is the core trick, applicable to any algorithm that accesses a unitary via controlled operations.

2. [[Parity Shifting via Spectral Multiplication in GQSP]] — multiplying a polynomial by $e^{i\theta}$ (equivalently, by $U$) to shift its Fourier parity from even to odd (or vice versa), then using [[Generalized Quantum Signal Processing (GQSP)|GQSP]]'s complex-polynomial freedom to accommodate the result.

3. [[Rudin Sum-of-Squares Completion with Parity Control]] — using Rudin's factorization of non-negative polynomials into sums of squares, with explicit tracking of parity through the root-pairing structure, to construct correction polynomials with prescribed even/odd character.

---

## References within this paper

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low–Chuang (2017)]] [Ref 9] — the standard QSP simulation this paper improves upon
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low–Chuang (2019)]] [Ref 10] — qubitization framework, block-encoding formalism
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén–Su–Low–Wiebe (2019)]] [Ref 11] — QSVT unifying framework
- Motlagh–Wiebe (arXiv:2308.01501) [Ref 12] — GQSP, the key ingredient enabling complex polynomial pairs
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney, Berry, Wiebe, et al. (2018)]] [Ref 15] — first used $U$-vs-$U^\dagger$ control to double phase for eigenvalue estimation; direct precursor to this work
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] [Ref 1] — first product-formula simulation
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry–Ahokas–Cleve–Sanders (2007)]] [Ref 2] — early product-formula simulation
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry–Childs (2012)]] [Ref 4] — quantum walk simulation
- Childs–Wiebe (2012) [Ref 5] — LCU simulation
- Berry–Childs–Cleve–Kothari–Somma (2014) [Ref 6] — truncated Taylor series
- Rudin (2000) [Ref 16] — sum-of-squares decomposition for non-negative polynomials

---

## Cross-links

### Paper notes
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the baseline this improves
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — the block-encoding and walk-operator framework used here
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — the broader QSVT framework
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — phase-doubling for eigenvalue estimation (the direct ancestor)
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — alternative approach to the parity obstruction (via [[Affine Spectrum Compression for QSP Parity Bypass|affine compression]])
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — phase-finding algorithms that would need GQSP extension
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — complementary $2\times$ improvement via randomisation

### Trick cards
- [[Jacobi-Anger Truncation for QSP Simulation]] — the approximation engine underlying both standard QSP and this paper
- [[Polynomial Completion via Sum-of-Squares]] — the completion procedure, here extended with parity constraints
- [[Affine Spectrum Compression for QSP Parity Bypass]] — alternative solution to the same parity obstruction
- [[Asymmetric Qubitization]] — related block-encoding generalisation
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]] — related complementary-polynomial construction
- [[Bessel Linearisation of Quantum Walk Phases]] — earlier Bessel-based approach to walk-phase transformation
- [[Directional Walk Control for Phase Doubling]] — new trick card from this paper
- [[Parity Shifting via Spectral Multiplication in GQSP]] — new trick card from this paper
- [[Rudin Sum-of-Squares Completion with Parity Control]] — new trick card from this paper
