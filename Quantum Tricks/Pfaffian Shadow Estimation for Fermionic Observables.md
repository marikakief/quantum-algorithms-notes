# Pfaffian Shadow Estimation for Fermionic Observables

> **Source:** Wan, Huggins, Lee, Babbush, arXiv:2207.13723
> **Tags:** #trick #classical-shadows #Pfaffian #fermionic #measurement #matchgate #covariance-matrix

## What it does

Computes the expectation value of any product of Majorana operators $\tilde{\gamma}_S$ with respect to a matchgate shadow sample in $O(|S|^3)$ time, using a single Pfaffian of a covariance-matrix submatrix.

## The trick

Given a matchgate shadow sample $(Q, b)$ — where $U_Q$ is the applied matchgate circuit and $|b\rangle$ is the measurement outcome — the estimate for $\operatorname{tr}(\tilde{\gamma}_S \rho)$ is:

$$\operatorname{tr}\!\left(\tilde{\gamma}_S \, \mathcal{M}^{-1}(U_Q^\dagger |b\rangle\langle b| U_Q)\right) = \binom{2n}{|S|} \binom{n}{|S|/2}^{-1} \operatorname{pf}\!\left(i(Q' Q^T C_{|b\rangle} Q Q'^T)\big|_S\right)$$

where $Q' \in O(2n)$ defines the rotated Majorana basis ($\tilde{\gamma}_\mu = \sum_\nu Q'_{\mu\nu} \gamma_\nu$), and $C_{|b\rangle}$ is the covariance matrix of the computational basis state $|b\rangle$ (a block-diagonal matrix with $2 \times 2$ blocks $\begin{pmatrix} 0 & (-1)^{b_j} \\ (-1)^{b_j+1} & 0 \end{pmatrix}$).

The derivation: the inverse measurement channel $\mathcal{M}^{-1}$ acts on even operators by rescaling the $\Gamma_{2\ell}$ component by $\binom{2n}{2\ell}/\binom{n}{\ell}$. On a Gaussian state $U_Q^\dagger |b\rangle\langle b| U_Q$, the trace $\operatorname{tr}(\tilde{\gamma}_S \cdot \text{Gaussian})$ is given by Wick's theorem as a Pfaffian of the covariance matrix restricted to indices $S$.

The ratio $\binom{2n}{|S|}/\binom{n}{|S|/2}$ scales as $n^{|S|/2}$ for constant $|S|$ — this is the amplification factor from the inverse channel, and it also determines the variance.

## When to reach for it

- Estimating any $k$-local fermionic observable (products of $k$ Majorana operators, or equivalently, $k/2$-body fermionic operators) from matchgate shadow data.
- Particularly natural for covariance matrices, 1-RDMs (two Majorana operators, $|S| = 2$), and 2-RDMs ($|S| = 4$).
- Extends to arbitrary single-particle bases — the $Q'$ matrix lets you measure in any rotated Majorana frame without changing the shadow protocol.

## Complexity

- **Per sample:** $O(n^2)$ for the matrix product $Q' Q^T C_{|b\rangle} Q Q'^T$ (or $O(n |S|)$ if you only compute the $|S| \times |S|$ submatrix), plus $O(|S|^3)$ for the Pfaffian.
- **Variance:** $\binom{2n}{|S|}/\binom{n}{|S|/2} - 1 \approx n^{|S|/2}$ for constant $|S|$.
- **Total samples for $\varepsilon$ precision:** $O(n^{|S|/2} \log(M/\delta) / \varepsilon^2)$ via median-of-means, where $M$ is the number of observables and $\delta$ is the failure probability.

## Caveat

Only works for even $|S|$ (products of an even number of Majorana operators). Odd products are in $\Gamma_{\text{odd}}$, which is outside the image of the matchgate measurement channel. This is not a practical limitation for fermionic observables — physical observables conserve parity — but rules out estimating, e.g., single Majorana expectation values directly.

The variance grows polynomially with $|S|$, so this is only practical for $|S| = O(1)$. For large $|S|$ (e.g., the full parity operator $\gamma_{[2n]}$ with $|S| = 2n$), the variance is exponential.

## Related notes

- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]]
- [[Matchgate 3-Design for Classical Shadows]] — ensures the variance formula holds for both discrete and continuous ensembles
- [[Generating-Function Pfaffian Trick for Gaussian Overlaps]] — extension to non-local observables (Gaussian fidelities)
- [[Pauli Expectation Value Estimation]] — qubit-basis alternative; matchgate shadows are better adapted to fermionic structure
- [[Basis Rotation Grouping for VQE Measurement]] — deterministic alternative for fermionic observables with $O(N)$ circuits
