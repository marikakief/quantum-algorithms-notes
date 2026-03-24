# Hamiltonian Term Truncation for VQE

> **Source:** McClean, Romero, Babbush & Aspuru-Guzik, arXiv:1509.04279
> **Tags:** #trick #VQE #measurement #variance-reduction #quantum-chemistry

## What it does

Reduces the number of Hamiltonian terms that need to be measured in [[Term-by-Term Expectation Estimation|term-by-term expectation estimation]] by dropping terms with small coefficients. The energy error is bounded by the sum of dropped coefficient magnitudes.

## The trick

For $H = \sum_{\alpha=1}^M h_\alpha O_\alpha$ with $\|O_\alpha\| \leq 1$, partition the terms into a "kept" set $K$ and a "dropped" set $D$ based on a threshold $\delta$:

- **Keep** terms with $|h_\alpha| \geq \delta$
- **Drop** terms with $|h_\alpha| < \delta$

The truncated Hamiltonian $\tilde{H} = \sum_{\alpha \in K} h_\alpha O_\alpha$ satisfies:

$$|\langle H \rangle - \langle \tilde{H} \rangle| \leq \sum_{\alpha \in D} |h_\alpha| \leq |D| \cdot \delta$$

For molecular Hamiltonians in chemistry, the two-electron integrals $h_{pqrs}$ have a heavy tail: most are small. Truncating terms below $\delta$ can eliminate a large fraction of the $O(N^4)$ terms while introducing only a small energy bias.

## When to reach for it

- Any [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]] calculation where the number of Hamiltonian terms $M$ dominates the measurement budget
- Chemistry Hamiltonians where many two-electron integrals $h_{pqrs}$ are negligible
- As a preprocessing step before [[Importance Sampling over Hamiltonian Terms|importance sampling]] or [[Commuting-Group Parallelisation via Graph Colouring|commuting group measurement]]

## Complexity

Measurement cost with $|K|$ kept terms: $O(|K| \cdot \|h_K\|_{\max}^2 / p^2)$ or $O(\|h_K\|_1^2 / p^2)$ with importance sampling. The saving is proportional to the fraction of terms dropped.

Classical preprocessing: $O(M \log M)$ to sort and threshold the coefficients.

## Caveat

The truncation introduces a **systematic bias**, not random error — the variational upper bound property is lost for the dropped terms. If you need the total energy to chemical accuracy ($\sim 1.6$ mHa), you must ensure $\sum_{\alpha \in D} |h_\alpha|$ is below that threshold.

For large basis sets, the truncation error can be significant. Smarter strategies (e.g., grouping small terms into effective operators) can do better than naive truncation.

## Related notes

- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]
- [[Term-by-Term Expectation Estimation]]
- [[Importance Sampling over Hamiltonian Terms]]
- [[Commuting-Group Parallelisation via Graph Colouring]]
