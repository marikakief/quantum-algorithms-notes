
> **Source:** Huang, Broughton, Mohseni, Babbush, Boixo, Neven, McClean, arXiv:2011.01938
> **Tags:** #trick #quantum-ml #kernel-methods #benchmarking #learning-theory

## What it does
Efficiently constructs a label function that maximally separates two kernel methods — producing a dataset where one method provably outperforms the other by the largest possible margin.

## The trick
Given quantum kernel matrix $K_Q$ and classical kernel matrix $K_C$ (both $N \times N$), find labels $y \in \mathbb{R}^N$ that maximize:

$$\frac{s_C}{s_Q} = \frac{y^T (K_C)^{-1} y}{y^T (K_Q)^{-1} y}$$

This is a generalized eigenvalue problem. The solution is $y = \sqrt{K_Q}\, v$, where $v$ is the top eigenvector of:

$$\sqrt{K_Q}(K_C)^{-1}\sqrt{K_Q}$$

with eigenvalue $g^2 = \|\sqrt{K_Q}(K_C)^{-1}\sqrt{K_Q}\|_\infty$

This guarantees $s_Q = 1$ and $s_C = g^2$, saturating the [[Geometric Difference Test for Quantum ML Advantage|geometric difference]] bound exactly.

**With regularization $\lambda > 0$:** Solve for the top eigenvector of $\sqrt{K_Q}\sqrt{K_C}(K_C + \lambda I)^{-2}\sqrt{K_C}\sqrt{K_Q}$ instead. Choose $\lambda$ such that the training error bound $g_{\text{tra}}^2 s_Q$ stays small (e.g., $< 0.002$).

**For classification:** Convert real-valued $y$ to binary by thresholding at the median, optionally adding 10% label noise for robustness.

## When to reach for it
- Benchmarking a proposed quantum ML model: construct the hardest possible dataset for classical methods and measure actual performance gap
- Validating the [[Geometric Difference Test for Quantum ML Advantage|geometric difference]] framework: if the constructed dataset doesn't show advantage matching $g_{CQ}$, something is wrong with the model
- Designing quantum ML experiments: generate challenge datasets that are easy for quantum devices but hard for classical ML
- Producing verifiable QML benchmarks: the quantum device generates easy-to-verify labels on a dataset designed to be hard for classical models

## Complexity
$O(N^3)$ for the eigenvalue problem (standard linear algebra). Requires pre-computed kernel matrices.

## Caveat
- Produces **adversarial/engineered** datasets, not naturally occurring ones. Strong performance on these datasets doesn't imply advantage on practical problems.
- The construction requires the classical kernel matrix to be invertible (or regularized). Near-singular $K_C$ can create numerical instability.
- Optimizes against a specific classical kernel $K_C$. A different classical method might perform better on the constructed dataset — though empirically, advantage is stable across methods including random forests.
- The advantage diminishes as $N$ grows (all methods converge with enough data).

## Related notes
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
- [[Geometric Difference Test for Quantum ML Advantage]]
- [[Projected Quantum Kernel via Local Observables]]
