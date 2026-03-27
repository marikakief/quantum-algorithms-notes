
> **Tags:** #trick #phase-estimation #chebyshev #qeve
> **Source:** arXiv:2401.06240 (Quantum eigenvalue processing, FOCS 2024)

## What it does

Estimates eigenvalues of diagonalizable matrices with real spectra in $[-1,1]$ with Heisenberg-limited $O(1/\varepsilon)$ scaling in precision, using a Chebyshev history state rather than [[Hamiltonian simulation]]. Works for non-normal matrices where QPE on $e^{-iAt}$ would be complicated or meaningless (since $A$ might not be unitary or even normal).

## The trick

1. **Build the Chebyshev history state:** [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] $|P_j\rangle = \sum_{k=0}^{d-1} |k\rangle |T_k(A)|\psi_j\rangle\rangle$ for an eigenstate $|\psi_j\rangle$ with eigenvalue $\lambda_j \in [-1,1]$ (see [[History-State Encoding with Unary Clock]]).

2. **Exploiting the Chebyshev identity:** $T_k(\lambda_j) = \cos(k\arccos(\lambda_j)) = \cos(k\theta_j)$ where $\theta_j = \arccos(\lambda_j) \in [0, \pi]$.

3. **Fourier analysis:** The $k$-register of $|P_j\rangle$ contains the discrete cosine series $\{\cos(k\theta_j)\}$. Applying a QFT (or DCT) on the $k$-register gives a sharp peak at $\pm\theta_j/(2\pi)$.

4. **Readout:** Measure the $k$-register in the Fourier basis → read off $\theta_j$ to precision $1/d$ → recover $\lambda_j = \cos(\theta_j)$.

Using $d = O(1/\varepsilon)$ Chebyshev terms achieves precision $\varepsilon$ in $\lambda_j$ — Heisenberg-limited, since the $d$ terms play the same role as $d$ time steps in QPE.

The history state is built via the [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]] trick, costing $O(\kappa_V/\varepsilon)$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries total.

## Comparison to QPE

Standard QPE on a Hermitian operator $H$ (with $A = e^{-iH}$ or similar) requires simulating $e^{-iAt}$ for $t = O(1/\varepsilon)$, which costs $O(1/\varepsilon)$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries. This achieves the same Heisenberg limit via a different route — useful when simulating $e^{-iAt}$ is expensive or $A$ has no natural unitary evolution.

## When to reach for it

- Non-Hermitian matrices with real eigenvalues (symmetrizable operators, PT-symmetric Hamiltonians, iteration matrices of iterative solvers)
- Settings where [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of $A$ is cheap but simulation of $e^{-iAt}$ is not
- Ground state energy estimation for matrices beyond the Hermitian class

## Complexity

$O(\kappa_V / \varepsilon)$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries, where $\kappa_V$ is the condition number of the eigenbasis matrix $V$ in $A = V\Lambda V^{-1}$. For Hermitian $A$, $\kappa_V = 1$ and this recovers the standard Heisenberg limit.

## Caveat

- Success amplitude in extracting $|\psi_j\rangle$ from an initial state is $O(1/\kappa_V)$, requiring $O(\kappa_V)$ amplitude amplification rounds.
- Works for diagonalizable matrices; Jordan-block structure (defective matrices) breaks the Chebyshev identity since generalized eigenvectors don't have eigenvalues in the usual sense.
- Precision in $\lambda_j = \cos(\theta_j)$ degrades near $\theta_j \approx 0$ or $\pi$ (i.e., $\lambda_j \approx \pm 1$), where $d(\cos\theta)/d\theta \approx 0$ — analogous to QPE precision loss near eigenphase $0$ or $\pi$.

## Related Paper Notes
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]]
