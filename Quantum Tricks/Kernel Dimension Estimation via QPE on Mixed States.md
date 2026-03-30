# Kernel Dimension Estimation via QPE on Mixed States

> **Source:** Lloyd, Garnerone, Zanardi, arXiv:1408.3106 (2016)
> **Tags:** #trick #phase-estimation #kernel #DQC1

## What it does
Estimates the dimension of the kernel of a sparse Hermitian matrix as a fraction of the total dimension, using [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|quantum phase estimation]] on the maximally mixed state.

## The trick
Given a $D \times D$ sparse Hermitian matrix $H$:

1. Prepare the maximally mixed state $\rho = I/D$
2. Run QPE for $e^{iHt}$ on $\rho$, producing eigenvalue estimates in an ancilla register
3. Measure the ancilla: probability of observing eigenvalue $\approx 0$ is $\dim(\ker H) / D$
4. Repeat $M = O(1/\epsilon^2)$ times to estimate this probability to additive error $\epsilon$

The mixed state $\rho$ can be prepared by starting with a uniform superposition $|\psi\rangle = \frac{1}{\sqrt{D}} \sum_i |i\rangle$, copying the register, and tracing out the copy. Alternatively, in the [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|DQC1]] model, one uses a single clean qubit with the rest maximally mixed.

QPE precision must resolve the spectral gap: need precision $\sim \lambda_{\min}$ to distinguish zero from non-zero eigenvalues, where $\lambda_{\min}$ is the smallest non-zero eigenvalue.

## When to reach for it
- Estimating Betti numbers (kernel dimension of combinatorial Laplacians)
- Estimating nullity of any sparse operator with a promise on the spectral gap
- Rank estimation (via rank-nullity: $\text{rank} = D - \dim \ker$)
- Any problem reducible to counting eigenvalues below a threshold ([[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes|low-lying spectral density estimation]])

## Complexity
$O\left(\frac{T_{\text{sim}}}{\lambda_{\min} \cdot \epsilon^2}\right)$ total, where $T_{\text{sim}}$ is the cost of one Hamiltonian simulation step and $\epsilon$ is the additive error on $\dim(\ker H)/D$.

## Caveat
Only estimates the *normalised* kernel dimension. When $D$ is exponentially large and $\dim \ker H$ is polynomially bounded, the fraction $\dim(\ker H)/D$ is exponentially small and cannot be resolved in polynomial time. This is precisely the limitation identified in [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes|Schmidhuber & Lloyd (2023)]]. For multiplicative estimation of $\dim \ker H$, one needs [[Amplitude Estimation Applied to Random Walk PDE Solvers|amplitude estimation]], which changes the scaling — see [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry et al. (2024)]].

## Related notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]]
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]]
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
