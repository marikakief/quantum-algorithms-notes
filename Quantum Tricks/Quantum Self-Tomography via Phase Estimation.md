# Quantum Self-Tomography via Phase Estimation

> **Source:** Lloyd, Mohseni & Rebentrost, arXiv:1307.0401 (2014)
> **Tags:** #trick #tomography #PCA #phase-estimation #quantum-ML

## What it does

Uses multiple copies of an unknown density matrix $\rho$ to decompose any state into $\rho$'s eigenbasis, revealing the principal components (dominant eigenvectors and eigenvalues) in time $O(R \log d)$ for rank-$R$ matrices in dimension $d$.

## The trick

1. Use [[Density Matrix Exponentiation via Partial Swap|density matrix exponentiation]] to implement controlled-$e^{-i\rho t}$
2. Apply [[Gapped Phase Estimation|phase estimation]] to $e^{-i\rho t}$ acting on input state $|\psi\rangle$:

$$
|\psi\rangle|0\rangle \to \sum_i \psi_i |\chi_i\rangle|\tilde{r}_i\rangle
$$

where $|\chi_i\rangle$ are eigenvectors of $\rho$ and $\tilde{r}_i \approx r_i$ are eigenvalue estimates.

3. **Self-analysis:** Using $\rho$ itself as input gives $\sum_i r_i |\chi_i\rangle\langle\chi_i| \otimes |\tilde{r}_i\rangle\langle\tilde{r}_i|$ — sampling yields the $i$-th principal component with probability $r_i$.

The state "participates in its own analysis" — the density matrix is simultaneously the Hamiltonian and the input.

## When to reach for it

- Learning properties of an unknown quantum state (quantum tomography)
- Finding dominant eigenvectors of a data covariance matrix (quantum PCA for ML)
- State discrimination: decompose in eigenbasis of $\rho - \sigma$ to classify
- Process tomography via the Choi-Jamiołkowski state

## Complexity

- Copies of $\rho$: $O(1/\epsilon^3)$ for eigenvalue precision $\epsilon$
- Time: $O(R \log d / \epsilon^3)$
- Classical PCA: $O(d \cdot \text{poly}(R))$ — exponentially slower in $d$

## Caveat

Only reveals an $O(\log d)$-bit description of the eigenvectors (as quantum states). Can't read out all $d$ components — that would take $O(d)$ time. Useful when you need to *use* the eigenvectors (as quantum states) rather than *describe* them classically.

For classical data: dequantized by Tang (2018). The advantage is real for genuinely quantum data.

## Related notes

- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]]
- [[Density Matrix Exponentiation via Partial Swap]]
- [[Gapped Phase Estimation]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
