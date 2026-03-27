
> **Source:** Huggins, Wan, McClean, O'Brien, Wiebe, Babbush, arXiv:2111.09283 (Appendix H); extends Gilyen, Arunachalam, Wiebe (2019)
> **Tags:** #trick #gradient-estimation #quantum-algorithms #measurement #QFT

## What it does

Generalises the Gilyen-Arunachalam-Wiebe quantum gradient estimation algorithm to handle gradient components with different magnitudes. Instead of allocating the same number of qubits to each coordinate's index register, it uses $n_i = \lceil \log(12 z_i / \varepsilon) \rceil$ qubits for coordinate $i$, where $z_i$ bounds $|\partial_i f|$. This replaces the worst-case $B_{\max}\sqrt{M}$ query scaling with the tighter $\ell_2$-norm $\|\vec{B}\|_2$.

## The trick

In the standard gradient algorithm (Jordan/Gilyen et al.), all $M$ coordinates of the input register have the same precision — $n$ qubits each. This wastes resources when gradient components differ in magnitude: coordinates with small gradients get over-resolved, coordinates with large gradients dominate the error.

The fix: allocate $n_i = \lceil \log(12 z_i / \varepsilon) \rceil$ qubits to the $i$-th index register, where $|(\nabla f)_i| \leq z_i$. The algorithm is otherwise unchanged:

1. Prepare a uniform superposition over the non-uniform grid $G = G_{n_1} \times \cdots \times G_{n_M}$
2. Apply the phase oracle $O(S)$ times with $S = 4/(\varepsilon r)$
3. Apply inverse QFT to each register separately (each with its own precision)
4. Measure and extract gradient components

The key analytical step: replace the uniform derivative bound $|\partial_\alpha f| \leq c^k k^{k/2}$ with the product bound $|\partial_\alpha f| \leq z_{\alpha_1} \cdots z_{\alpha_k}$ for $\alpha \in [M]^k$. This leads to a Hoeffding-inequality argument showing that the finite-difference approximation error concentrates with a norm dependence of $\|\vec{z}\|_2$ rather than $z_{\max}\sqrt{M}$.

For the [[Gradient Encoding for Multi-Observable Estimation|multi-observable estimation]] application, $z_j = 2B_j$ where $B_j = \|O_j\|$, giving:

$$\text{Query complexity} = \tilde{O}\!\left(\frac{\sqrt{\sum_j B_j^2}}{\varepsilon}\right) = \tilde{O}\!\left(\frac{\|\vec{B}\|_2}{\varepsilon}\right)$$

## When to reach for it

- Estimating expectation values of observables with heterogeneous norms. Example: a molecular Hamiltonian where some terms (Coulomb) have large coefficients and others (exchange) are small.
- Estimating expectation values to different precisions $\varepsilon_j$: rescale each observable by $\varepsilon_j / \varepsilon$ to convert non-uniform precision requirements into non-uniform norm bounds.
- Any application of quantum gradient estimation where the function's partial derivatives are not uniformly bounded.

## Complexity

- **Queries to phase oracle:** $\tilde{O}(\|\vec{z}\|_2 / \varepsilon)$
- **Qubits:** $O(\sum_i \log(z_i / \varepsilon))$ for index registers
- **Gates:** $O(\log(M/\delta) \sum_i \log(z_i/\varepsilon) \log\log(z_i/\varepsilon))$ for the inverse QFTs

## Caveat

Only helps when the $z_i$ vary substantially. If all $z_i \approx z_{\max}$, this reduces to the standard algorithm with no improvement. The analysis also requires the product-form derivative bound $|\partial_\alpha f| \leq \prod z_{\alpha_i}$, which holds for the expectation-value function but may not hold for arbitrary smooth functions.

## Related notes
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]
- [[Gradient Encoding for Multi-Observable Estimation]]
- [[Probability Oracle from Hadamard Test]]
