# Normal Matrix Fourier Decomposition via Root-of-Unity Unitary

> **Source:** Motlagh and Wiebe, arXiv:2308.01501 (2023)
> **Tags:** #trick #GQSP #normal-matrices #convolution #QFT #block-encoding

## What it does

Any $N \times N$ normal matrix $A$ that is a degree-$d$ polynomial of a root-of-unity diagonal unitary $U_\omega$ can be block-encoded using $O(d \log N)$ 1- and 2-qubit gates via [[Generalized Quantum Signal Processing (GQSP)]]. This handles diagonal matrices and circulant (convolution) matrices as special cases.

## The trick

Let $\{|\lambda_j\rangle\}_{j=0}^{N-1}$ be the eigenbasis of a normal matrix $A$, with eigenvalues $\lambda_j$. Define the root-of-unity unitary:

$$U_\omega = \sum_{j=0}^{N-1} \omega_N^j |\lambda_j\rangle\langle\lambda_j|, \quad \omega_N = e^{2\pi i / N}$$

**Key decomposition:** If $\lambda_j$ can be expressed as a degree-$d$ polynomial in $\omega_N^j$, i.e., $\lambda_j = P(\omega_N^j)$ for some $P \in \mathbb{C}[z]$ with $\deg P = d$ and $|P| \leq 1$, then:

$$A = P(U_\omega)$$

and $A$ is directly implementable via GQSP using $d$ controlled-$U_\omega$ operations and $d+1$ SU(2) rotations.

**Diagonal matrices:** $A = \mathrm{diag}(\lambda_0, \dots, \lambda_{N-1})$ where $\lambda_j = P(\omega_N^j)$ is a DFT-style filter. Eigenbasis is the computational basis. Implementing $U_\omega$ costs $O(\log N)$ gates (it's just phase kickback from an $n$-qubit phase oracle).

**Circulant matrices:** For cyclic convolution by a filter $f_0, \dots, f_{d-1}$, the matrix is circulant and diagonalised by the [[Quantum Fourier Transform Circuit|QFT]]:
$$A = Q \cdot \mathrm{diag}(\hat{f}) \cdot Q^\dagger$$

where $Q$ is the QFT. After conjugating by $Q$, the diagonal $\mathrm{diag}(\hat{f})$ is a polynomial of $U_\omega$ of degree $d$. Full circuit: $Q^\dagger$ (QFT), then GQSP on $U_\omega$, then $Q$ (inverse QFT). Total: $O(d \log N + \log^2 N)$ gates ($\log^2 N$ for the QFT).

## When to reach for it

- Implementing a quantum convolution: filter of length $d$ applied to an $N$-dimensional state with $O(d \log N + \log^2 N)$ gates instead of $O(Nd)$ classically.
- Block-encoding diagonal operators expressed as low-degree Fourier series (e.g., phase operators, quadrature functions on a periodic domain).
- Any quantum signal processing task where the input oracle has periodic eigenvalue structure aligned with $N$th roots of unity.

## Complexity

| Task | Gate count |
|---|---|
| Degree-$d$ diagonal filter, $N$-dim space | $O(d \log N)$ |
| Circulant convolution, filter length $d$ | $O(d \log N + \log^2 N)$ |
| General normal matrix via QFT eigenbasis | $O(d \log N + \log^2 N)$ |

## Caveat

Requires the matrix to be a low-degree polynomial of $U_\omega$. The degree $d$ equals the filter length or spectral bandwidth. For dense/unstructured normal matrices, $d = N$ and this gives no improvement over direct synthesis. The method also requires knowing the eigenbasis change-of-basis efficiently — QFT works for circulant; other normal matrices may need costly $Q$ circuits.

The polynomial $P$ must satisfy $|P| \leq 1$ on the unit circle (required by unitarity). For matrices with $\|\lambda_j\| > 1$, normalise first and absorb the normalisation into the block-encoding constant.

## Related notes

- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]
- [[Generalized Quantum Signal Processing (GQSP)]]
- [[Quantum Fourier Transform Circuit]]
- [[FFT-Based Complementary Polynomial Optimisation]]
