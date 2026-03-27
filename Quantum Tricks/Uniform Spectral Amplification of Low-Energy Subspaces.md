# Uniform Spectral Amplification of Low-Energy Subspaces

> **Source:** Low & Chuang, arXiv:1707.05391  
> **Tags:** #trick #spectral-amplification #QSP #hamiltonian-simulation #low-energy

## What it does

Reduces the effective normalization for simulating a low-energy subspace from $\alpha$ to $\alpha\Delta$ using only $O(\Delta^{-1/2})$ queries, where $\Delta$ is the fractional width of the energy window. This is a quadratic improvement over the naive $O(\Delta^{-1})$ cost — the same kind of quadratic advantage that [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|spectral gap amplification]] achieves, but with the spectrum preserved uniformly (no distortion of relative energy spacings).

## The trick

Given Hermitian standard-form-$(\hat{H}, \alpha, \hat{U}, d)$ with eigenvalues $\hat{H}/\alpha|{\lambda}\rangle = \lambda|\lambda\rangle$, target the low-energy subspace $\lambda \in [-1, -1+\Delta]$.

1. Construct a polynomial $p_{\text{gap},\Delta,n}(x)$ that approximates the function:
$$f_{\text{gap},\Delta}(x) = \frac{x + 1 - \Delta}{\Delta}, \quad x \in [-1, -1+\Delta]$$
while staying bounded by 1 on $[-1,1]$.

2. The quadratic advantage comes from the boundary behaviour of polynomials: Markov's inequality says a degree-$n$ polynomial bounded by 1 on $[-1,1]$ has gradient at most $n^2$ at the boundary. Chebyshev polynomials $T_n$ achieve this maximum at $x = \pm 1$. So near the boundary, a degree-$n$ polynomial can have slope $O(n^2)$, meaning you need only $n = O(\Delta^{-1/2})$ to stretch an interval of width $\Delta$ by factor $\Delta^{-1}$.

3. Apply [[Flexible QSP via A-Component Cancellation|flexible QSP]] with this polynomial to encode $\hat{H}_{\text{amp}}/(\Delta\alpha) \approx (\hat{H} + \alpha(1-\Delta)\hat{I})/(\Delta\alpha)$ in standard-form with normalization $\Delta\alpha$.

Total cost: $O(\Delta^{-1/2}\log^{3/2}(1/(\Delta\epsilon)))$ queries.

The factor $\log^{3/2}$ comes from the polynomial construction (Appendix B of the paper), which uses error function approximations to the sign function combined with Chebyshev truncation.

## When to reach for it

- Low-energy [[Hamiltonian simulation]]: ground state dynamics, low-lying excitations, quantum chemistry near the ground state.
- Adiabatic computation where you only care about the low-energy sector of a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoded]] Hamiltonian.
- This is the conceptual ancestor of [[SOSSA — Sum-of-Squares Spectral Amplification|SOSSA]] and [[SOS Spectral Amplification|SOS spectral amplification]] — those later techniques make $\Delta$ small by tightening the lower bound via SOS decompositions.

## Complexity

Combined with [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization-based simulation]]:
$$O\!\left(t\alpha\sqrt{\Delta}\log^{3/2}\!\left(\frac{t\alpha}{\epsilon}\right) + \Delta^{-1/2}\log^{5/2}\!\left(\frac{t\alpha}{\epsilon}\right)\right) \text{ queries}$$

For the amplification step alone: $O(\Delta^{-1/2}\log^{3/2}(1/(\Delta\epsilon)))$ queries.

## Caveat

- Only the low-energy subspace is faithfully simulated. Eigenvalues outside $[-1, -1+\Delta]$ are mapped to something bounded but otherwise uncontrolled.
- The odd symmetry of the polynomial means the high-energy subspace $[1-\Delta, 1]$ is also amplified. So this is really "boundary subspace" amplification.
- Most useful when $\alpha \approx \|\hat{H}\|$ (i.e., the block-encoding normalization is close to the spectral norm). For frustration-free Hamiltonians this is natural; for others, the gap between $\alpha$ and $\|\hat{H}\|$ eats into the advantage.

## Related notes
- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
- [[SOS Spectral Amplification]]
- [[Rectangular Block-Encoding for Spectrum Amplification]]
- [[Flexible QSP via A-Component Cancellation]]
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]
- [[Frustration-Free Gap Amplification via Walk Operator]]
