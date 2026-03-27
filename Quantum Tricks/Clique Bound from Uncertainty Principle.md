
> **Source:** King, Gosset, Kothari, Babbush, arXiv:2404.19211
> **Tags:** #trick #uncertainty-principle #anticommutativity #commutation-graph #shadow-tomography #structural-bound

## What it does

Bounds the clique number of the commutation graph of "significant" Pauli observables — those with large expectation values in a quantum state — using only the uncertainty principle. This structural constraint is what makes two-copy shadow tomography work.

## The trick

**The uncertainty relation (Lemma 14):** If $P_1, \ldots, P_m$ are pairwise anticommuting Paulis, then for any state $\rho$:

$$\sum_{j=1}^{m} \mathrm{tr}(P_j \rho)^2 \leq 1$$

The proof is a one-liner: define $Q = \sum_j a_j P_j$ where $a_j = \mathrm{tr}(P_j \rho)$. Anticommutativity gives $Q^2 = (\sum_j a_j^2) I$. The variance inequality $\mathrm{tr}(Q\rho)^2 \leq \mathrm{tr}(Q^2 \rho)$ then gives $(\sum_j a_j^2)^2 \leq \sum_j a_j^2$, so $\sum_j a_j^2 \leq 1$.

**Application to shadow tomography:** After [[Bell Difference Sampling for Magnitude Estimation|Bell sampling]], the significant set $S_\epsilon$ satisfies $|\mathrm{tr}(\rho P)| \geq \epsilon/2$ for all $P \in S_\epsilon$. A clique in $G(S_\epsilon)$ is a pairwise anticommuting subset. If the clique has size $\omega$, then $\omega \cdot \epsilon^2/4 \leq 1$, giving:

$$\omega \leq 4/\epsilon^2$$

This bounded clique number is the key input to all subsequent coloring arguments. It means the post-Bell-sampling commutation graph, while potentially large, is structurally tame — no vertex neighbourhood is too dense.

## When to reach for it

- As the structural bridge between Bell sampling and graph coloring in two-copy shadow tomography.
- When analyzing the commutation structure of observables that are simultaneously non-negligible on a quantum state.
- More generally: whenever you need to argue that a "large" set of Pauli observables cannot be too anticommuting.

## Complexity

No computational cost — this is a structural property, not an algorithm. It holds with high probability conditioned on the Bell sampling estimates being accurate.

## Caveat

The bound $\omega \leq 4/\epsilon^2$ is on the *clique number* (integer LP), not the *fractional clique number* (LP relaxation). Conjecture 13 of the paper asks whether the fractional clique number is also $O(1/\epsilon^2)$. If true, LP duality would immediately give fractional chromatic number $O(1/\epsilon^2)$, resolving the triply efficient shadow tomography question for all Pauli subsets. The gap between clique number and fractional chromatic number is where the difficulty lives.

The bound also says nothing about which specific Paulis are in $S_\epsilon$ — only that they can't be too anticommuting. The actual structure of $S_\epsilon$ depends on $\rho$ in a potentially complicated way.

## Related notes
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
- [[Bell Difference Sampling for Magnitude Estimation]]
- [[Fractional Coloring of Commutation Graphs]]
- [[Chi-Bounded Induction for Majorana Monomials]]
