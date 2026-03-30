# Row-by-Row MPS Contraction for 2D Tensor Networks

> **Source:** Verstraete and Cirac, arXiv:cond-mat/0407066
> **Tags:** #trick #tensor-networks #PEPS #classical-simulation #2D-systems

## What it does
Approximately contracts a 2D tensor network (e.g., a PEPS overlap $\langle\Psi|O|\Psi\rangle$) by treating it as a sequence of matrix product operator (MPO) applications on an MPS, compressing at each step.

## The trick
A 2D PEPS expectation value $\langle\Psi|O|\Psi\rangle$ can be written as a product of row transfer matrices. The bottom row, once contracted horizontally, forms an MPS $|U_1\rangle$ in the vertical bond indices. Each subsequent row acts as an MPO $U_k$. The top row gives $\langle U_{N_v}|$.

The expectation value is $\langle U_{N_v}| U_{N_v-1} \cdots U_2 |U_1\rangle$.

The problem: applying MPO $U_k$ to MPS $|U_{k-1}\rangle$ multiplies the bond dimension by $D^2$. After $k$ rows, the bond dimension would be $D^{2k}$ — exponential.

The fix: after each MPO application, find the MPS $|\tilde{U}_k\rangle$ of fixed bond dimension $D_f$ that best approximates $U_k |\tilde{U}_{k-1}\rangle$ by minimising $\| U_k |\tilde{U}_{k-1}\rangle - |\tilde{U}_k\rangle \|^2$. This is done via [[Variational Tensor Optimisation via Site-by-Site Sweeping|site-by-site sweeping]]: fix all MPS tensors except one, solve the resulting linear system (the cost function is quadratic in the free tensor), then move to the next site. Sweep back and forth until convergence.

The approximation error is exactly computable at each step, and $D_f$ can be increased if the error is too large.

## When to reach for it
- Computing expectation values of any 2D [[Projected Entangled Pairs as Variational Ansatz|PEPS]]
- Contracting 2D tensor networks arising from partition functions, classical statistical mechanics, or quantum circuit simulation
- The boundary MPS method: many 2D tensor network algorithms (iPEPS, variational PEPS) use this as their inner loop

## Complexity
Each sweep step costs $O(N_h D_f^3 D^4)$ (for a single row compression). There are $N_v$ rows, so the total cost per expectation value is $O(N_h N_v D_f^3 D^4)$ times the number of sweeps per compression. The accuracy is controlled by $D_f$.

## Caveat
This is an uncontrolled approximation — there's no rigorous bound on how the compression error accumulates across rows. In practice, the error tends to be well-behaved for gapped systems but can grow for critical systems or frustrated models where the boundary MPS develops long-range correlations.

Exact contraction of 2D tensor networks is #P-hard, so some approximation is unavoidable. This method trades exact answers for polynomial cost.

## Related notes
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]]
- [[Projected Entangled Pairs as Variational Ansatz]]
- [[Variational Tensor Optimisation via Site-by-Site Sweeping]]
- [[MPS Canonical Form for Efficient State Tracking]]
