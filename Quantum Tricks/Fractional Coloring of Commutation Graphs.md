# Fractional Coloring of Commutation Graphs

> **Source:** King, Gosset, Kothari, Babbush, arXiv:2404.19211
> **Tags:** #trick #graph-coloring #shadow-tomography #measurement-grouping #Pauli-learning #commutation-graph

## What it does

Reduces single-copy Pauli shadow tomography to a fractional coloring problem on the commutation graph of the observable set. The sample complexity of the resulting protocol equals the fractional chromatic number (times log factors), providing a unified framework that encompasses both random Pauli basis measurements and deterministic grouping strategies.

## The trick

Define the **commutation graph** $G(S)$ for a set $S \subseteq \mathcal{P}(n)$: vertices are Paulis, edges connect anticommuting pairs. An independent set in $G(S)$ is a commuting subset — measurable simultaneously via a single Clifford basis.

A **fractional coloring** of size $\chi$ is a probability distribution $q$ over independent sets such that every vertex appears with probability $\geq 1/\chi$. This is the LP relaxation of ordinary graph coloring.

The learning protocol: repeat $N = O(\chi(\log|S|)/\epsilon^2)$ times:
1. Sample an independent set $I \sim q$
2. Measure $\rho$ in the Clifford basis diagonalizing all Paulis in $I$
3. Record outcomes for each $P \in I$

Post-processing: for each $P \in S$, compute the median-of-means over all rounds where $P$ was measured. The probability that $P$ is measured in any round is $\geq 1/\chi$, so after $N$ rounds, each $P$ has $\Omega((\log|S|)/\epsilon^2)$ samples — enough for $\epsilon$-accurate estimation.

**Why this matters:** Deterministic Pauli grouping strategies (as used in [[Basis Rotation Grouping for VQE Measurement|VQE measurement optimization]]) correspond to *proper* graph colorings — a special case where the distribution is uniform over color classes. Fractional colorings are strictly more powerful: $\chi_f(G) \leq \chi(G)$, sometimes much smaller. The random Pauli basis protocol for $k$-local Paulis corresponds to a fractional coloring of size $3^k$, which is the fractional chromatic number of $G(\mathcal{P}^{(n)}_k)$.

## When to reach for it

- Designing measurement protocols for structured sets of Pauli observables.
- Analyzing the sample complexity of existing shadow tomography schemes (many implicitly define a fractional coloring).
- As the second stage of a two-copy protocol: after [[Bell Difference Sampling for Magnitude Estimation|Bell sampling]] identifies the significant set $S_\epsilon$, fractional coloring of $G(S_\epsilon)$ handles sign recovery.

## Complexity

$O(\chi_f(\log|S|)/\epsilon^2)$ copies of $\rho$, where $\chi_f$ is the fractional chromatic number of $G(S)$ (or $G(S_\epsilon)$ in the two-copy setting). Classical runtime: $O((T + n^3) \cdot \chi_f(\log|S|)/\epsilon^2)$ where $T$ is the time to sample from the fractional coloring.

The protocol also produces a compressed classical representation of $\rho$ of size $O(n^2 \chi_f (\log|S|)/\epsilon^2)$ bits, from which any Pauli expectation value in $S$ can be extracted in time proportional to the representation size.

## Caveat

Computing the fractional chromatic number is NP-hard for general graphs. The usefulness of this framework depends on having efficient fractional colorings for the specific commutation graph families of interest. For $k$-local Paulis, this is trivial ($\chi_f = 3^k$). For $k$-body fermionic operators, it requires the [[Chi-Bounded Induction for Majorana Monomials|chi-boundedness induction]], and the resulting polynomials grow rapidly with $k$.

## Related notes
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
- [[Basis Rotation Grouping for VQE Measurement]] — deterministic Pauli grouping as a special case
- [[Bell Difference Sampling for Magnitude Estimation]]
- [[Chi-Bounded Induction for Majorana Monomials]]
- [[Clique Bound from Uncertainty Principle]]
- [[Pauli Expectation Value Estimation]]
