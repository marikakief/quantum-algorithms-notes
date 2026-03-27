
> **Source:** Huang, Broughton, Mohseni, Babbush, Boixo, Neven, McClean, arXiv:2011.01938
> **Tags:** #trick #quantum-ml #kernel-methods #quantum-advantage #learning-theory

## What it does
Determines whether a quantum ML model can outperform a classical ML model on a given dataset, *before* looking at the labels — a function-independent pre-screening for quantum advantage.

## The trick
Given a classical kernel matrix $K_C$ and a quantum kernel matrix $K_Q$ (both $N \times N$, normalized to $\text{Tr}(K) = N$), compute the **geometric difference**:

$$g_{CQ} = g(K_C \| K_Q) = \sqrt{\left\|\sqrt{K_Q}(K_C)^{-1}\sqrt{K_Q}\right\|_\infty}$$

This satisfies the inequality $s_C \leq g_{CQ}^2 \, s_Q$, where $s_K$ is the model complexity controlling prediction error via $\epsilon \leq c\sqrt{s_K/N}$.

**Interpretation:**
- If $g_{CQ} \sim 1$: classical ML matches or beats quantum ML for **any** labeling of this dataset. No quantum advantage possible.
- If $g_{CQ} \gg \sqrt{N}$: there exists a labeling where quantum ML has a large prediction advantage over classical ML.

Computation requires SVD of $N \times N$ matrices — $O(N^3)$ classical time. In practice, compute $g_{CQ}$ for a suite of optimized classical kernels (Gaussian, linear, polynomial, neural tangent) and take the minimum.

With regularization $\lambda > 0$:

$$g_{\text{gen}} = \sqrt{\left\|\sqrt{K_Q}\sqrt{K_C}(K_C + \lambda I)^{-2}\sqrt{K_C}\sqrt{K_Q}\right\|_\infty}$$

Choose $\lambda$ to balance training error and generalization.

## When to reach for it
- Evaluating any proposed quantum ML model: compute $g_{CQ}$ before running expensive quantum experiments
- Designing quantum embeddings: optimize the encoding circuit to maximize $g_{CQ}$
- Benchmarking QML claims: if a paper reports quantum advantage without checking geometric difference, their result is suspect

## Complexity
$O(N^3)$ classical computation for SVD. Requires the kernel matrices $K_C$ and $K_Q$ — the quantum kernel matrix needs $O(N^2)$ quantum circuit evaluations or classical simulation.

## Caveat
- Requires white-box access to the quantum feature map (circuit specification)
- The geometric difference is data-dependent — it measures advantage potential for a specific set of input points, not universally
- A large $g_{CQ}$ means advantage *exists* for some labeling, not that the actual labels of interest exhibit it
- Minimizing $g_{CQ}$ over all efficient classical models is intractable; in practice, use a suite of standard classical kernels

## Related notes
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
- [[Projected Quantum Kernel via Local Observables]]
- [[Adversarial Dataset Construction via Generalized Eigenvalue Problem]]
