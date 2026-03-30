# Jordan's Lemma Subspace Decomposition for Alternating Measurements

> **Source:** Marriott & Watrous, arXiv:cs/0506068; Jordan (1875)
> **Tags:** #trick #Jordan-lemma #measurement #amplification #QMA #subspace

## What it does
Decomposes the Hilbert space into 2D invariant subspaces of two non-commuting projectors, enabling analysis of alternating measurements as independent coin flips within each block.

## The trick
Given two orthogonal projectors $P$ and $Q$ on a Hilbert space $\mathcal{H}$, Jordan's lemma (1875) states that $\mathcal{H}$ decomposes into a direct sum of 1D and 2D subspaces, each invariant under both $P$ and $Q$.

In each 2D block, the two projectors act as projections onto two lines in a plane, separated by some angle $\theta$. Alternating measurements with respect to $\{P, I-P\}$ and $\{Q, I-Q\}$ generate a random walk on two states within this 2D block, with transition probabilities $\cos^2\theta$ (same outcome) and $\sin^2\theta$ (different outcome).

**Application to QMA amplification:** Take $P = A^\dagger \Pi_1 A$ (acceptance after applying the verifier) and $Q = \Delta_1$ (workspace reset). For an eigenvector of the acceptance operator $Q_x$ with eigenvalue $p$, the alternating measurements produce independent Bernoulli trials with parameter $p$. After $N$ rounds, Chernoff bounds give exponential error reduction — without needing any additional witness qubits.

The general case (arbitrary witness $|\psi\rangle = \sum_j \alpha_j |\psi_j\rangle$): the subspaces for different eigenvalues are orthogonal, so the overall process decomposes into independent components, one per eigenvalue. The acceptance probability is $\sum_j |\alpha_j|^2 f(p_j)$ where $f$ is a binomial tail probability.

## When to reach for it
- **QMA amplification** without increasing witness length (the original application)
- **Quantum Metropolis rejection recovery**: alternating "would this be accepted?" and "is this the same energy?" measurements converge to the pre-measurement state — see [[Marriott-Watrous Iterative Rejection for Quantum Metropolis]]
- Any protocol where you need to extract information about an eigenvalue of some operator by repeatedly applying and "un-applying" a quantum circuit
- Analysing the convergence of iterative quantum procedures involving two non-commuting measurements

## Complexity
$O(1/(a-b)^2 \cdot r)$ applications of the verifier circuit $A$ and $A^\dagger$ to achieve error $2^{-r}$, where $a - b$ is the initial completeness-soundness gap.

## Caveat
The decomposition is non-constructive — you don't need to find the invariant subspaces, but the analysis depends on their existence. The independence of coin flips within each block relies on the measurements being exact projections; approximate measurements would introduce correlations. Also, this technique only works for two projectors — three or more projectors don't admit a clean simultaneous block diagonalisation.

## Related notes
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[Marriott-Watrous Iterative Rejection for Quantum Metropolis]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Trace Trick for QMA-to-PP Reduction]]
