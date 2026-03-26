# Parallelised Importance Sampling for Vector Estimation

> **Source:** O'Brien, Streif, Rubin, Santagati, Su, Huggins et al., arXiv:2111.12437
> **Tags:** #trick #measurement #importance-sampling #NISQ #estimation

## What it does

Optimises shot allocation across measurement bases to minimise the 2-norm of the error vector when estimating multiple expectation values simultaneously, rather than targeting each component independently.

## The trick

When estimating a vector of $3N_a$ expectation values $\langle dH/dR_i \rangle$ via tomography with basis rotations $\{Q_j\}$, the error on each component propagates as $\varepsilon_i^2 = \sum_j \sigma_{i,j}^2 / M_j$. The total squared error in the gradient vector is $\|\varepsilon\|_2^2 = \sum_i \varepsilon_i^2$.

Minimise $\|\varepsilon\|_2^2$ subject to $\sum_j M_j = M$ via Lagrange multipliers. The optimal shot allocation is:

$$M_j = M \frac{\sqrt{\sum_i \bar{\sigma}_{i,j}^2}}{\sum_{j'} \sqrt{\sum_i \bar{\sigma}_{i,j'}^2}}$$

yielding the total shot count:

$$M \leq \varepsilon^{-2} \left(\sum_j \sqrt{\sum_i \bar{\sigma}_{i,j}^2}\right)^2$$

The key advantage over **separate** estimation (where each force component gets its own independent shot budget) is that a single basis rotation $Q_j$ simultaneously constrains the error on *all* force components. Separate estimation costs $M \leq \varepsilon^{-2} (\sum_{i,j} \bar{\sigma}_{i,j})^2$, which is worse by the Cauchy-Schwarz gap between $\sum_j \sqrt{\sum_i x_{ij}^2}$ and $\sum_{i,j} x_{ij}$. In the best case (uniform variances across components), the saving is a factor of $3N_a$.

## When to reach for it

- Measuring a vector of observables from a single quantum state — forces, dipole moments, correlation function vectors, any situation where the error target is on the *norm* of the error rather than individual components.
- When measurement bases can be reused across observables (e.g., Pauli grouping, shadow tomography). If each observable requires its own basis (e.g., some factorised methods), the parallelisation gain disappears.
- Particularly useful when variance contributions $\sum_i \bar{\sigma}_{i,j}^2$ vary significantly across bases — importance sampling then concentrates shots on the high-variance bases.

## Complexity

The total shot count is $M = \varepsilon^{-2} \Gamma_2^{(\text{par})}$ where $\Gamma_2^{(\text{par})} = (\sum_j \sqrt{\sum_i \bar{\sigma}_{i,j}^2})^2$. Without importance sampling (uniform $M_j$), the cost is $M = \varepsilon^{-2} N_r \sum_{i,j} \bar{\sigma}_{i,j}^2$ where $N_r$ is the number of basis rotations. The saving from importance sampling can be up to a factor of $N_r$.

## Caveat

Requires estimates of the variance bounds $\bar{\sigma}_{i,j}$. In practice these are upper bounds (not exact), so the optimality is only with respect to the chosen bounds. The tightness of the bounds varies by method — fermionic shadow tomography has exact variance expressions, while Pauli and BRG methods use looser spectral-range bounds. Also, for factorised measurement schemes (like [[Double Factorization of Two-Electron Integrals|DF]]), basis rotations for different force components don't naturally overlap, so the parallelisation gain may be limited.

## Related notes
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]]
- [[Basis Rotation Grouping for VQE Measurement]]
- [[Pauli Expectation Value Estimation]]
- [[Gradient Encoding for Multi-Observable Estimation]]
