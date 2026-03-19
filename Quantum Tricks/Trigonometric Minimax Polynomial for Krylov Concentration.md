# Trigonometric Minimax Polynomial for Krylov Concentration

> **Source:** Epperly, Lin, and Nakatsukasa, arXiv:2110.07492 (SIAM J. Matrix Anal. Appl. 43(3), 2022), Lemma 3.3
> **Tags:** #trick #Krylov #Chebyshev #minimax #spectral-concentration

## What it does

Concentrates a unitary Krylov subspace's weight at a target eigenvalue (the ground state) while exponentially suppressing contributions from all other eigenvalues. The periodic analogue of Chebyshev polynomial concentration for real-line Krylov methods.

## The trick

In unitary Krylov methods, the basis vectors are $\phi_j = e^{ij\Delta t \hat{H}}\phi_0$, so the eigenvalues of $\hat{H}$ are mapped to phases $\theta_k = \Delta t \cdot E_k$ on the circle $(-\pi, \pi]$. Standard Chebyshev polynomials don't apply directly because the spectrum wraps around.

The fix: use **trigonometric polynomials** $p(\theta) = \sum_{|j| \leq k} c_j e^{ij\theta}$ of degree $\leq k$ that solve the minimax problem:

$$\beta(a, k) = \min_{\substack{p \in \mathcal{T}_k \\ p(0) = 1}} \max_{\theta \in (-\pi, \pi) \setminus (-a, a)} |p(\theta)|$$

The solution is $p^*(\theta) = T_k\!\left(1 + \frac{2(\cos\theta - \cos a)}{\cos a + 1}\right) \Big/ T_k\!\left(1 + \frac{2(1 - \cos a)}{\cos a + 1}\right)$, where $T_k$ is the standard Chebyshev polynomial of degree $k$.

The optimal suppression ratio satisfies:

$$\beta(a, k) \leq 2(1 + a)^{-k}$$

So with $a = \pi\Delta E_1/\Delta E_M$ (the spectral gap normalised by the bandwidth), the suppression decays exponentially in $k$.

## When to reach for it

- Analysing unitary Krylov subspace methods (QSD, quantum filter diagonalisation, quantum power method).
- Any setting where you need to understand how time-evolution Krylov subspaces approximate eigenstates.
- Proving exponential convergence rates for Krylov methods with periodic/circular spectra.

## The connection to standard Chebyshev analysis

On the real line, the analogous result uses Chebyshev polynomials $T_k$ directly: the Krylov residual for a matrix with spectral gap $\delta$ and spectral width $\Delta$ decays as $O((1 + 2\delta/\Delta)^{-k})$. The trigonometric version handles the $2\pi$-periodicity introduced by unitary evolution $e^{it\hat{H}}$, mapping the problem back to Chebyshev via a cosine change of variables.

## Complexity

For QSD with Krylov dimension $n = 2k+1$: the Rayleigh–Ritz error for the ground energy decays as

$$O\!\left(\frac{\Delta E_{N-1}(1 - |\gamma_0|^2)}{|\gamma_0|^2}\left(1 + \frac{\pi\Delta E_1}{\Delta E_{N-1}}\right)^{-2k}\right)$$

So the number of time steps to achieve Rayleigh–Ritz error $\delta$ is $k = O\!\left(\frac{\Delta E_{N-1}}{\Delta E_1}\log\frac{1}{\delta}\right)$.

## Caveat

- The exponential rate depends on $\Delta E_1/\Delta E_{N-1}$ — the ratio of spectral gap to spectral width. For gapless systems or systems with $\Delta E_1 \ll \Delta E_{N-1}$, convergence is slow.
- The $|\gamma_0|^{-2}$ prefactor means small initial overlap requires more timesteps or better state preparation.
- The bound counts Krylov dimension, not total quantum cost; each timestep requires running a Hadamard-test circuit with evolution time $|j|\Delta t$.

## Related notes
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]]
- [[Chebyshev Polynomial Spectral Projector]]
- [[Löwdin Thresholding for Noisy Generalized Eigenvalue Problems]]
- [[Approximate CDF from Hadamard Tests]]
