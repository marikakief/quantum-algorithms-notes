# Amplitude Estimation for Distribution Property Testing

> **Source:** Bravyi, Harrow, Hassidim, arXiv:0907.3920
> **Tags:** #trick #amplitude-estimation #distribution-testing #property-testing

## What it does
Converts distribution testing problems (uniformity, closeness, orthogonality) into quantum amplitude estimation problems, giving polynomial speedups over classical sampling.

## The trick
The classical approach to testing a distribution property samples $i$ from $p$, observes the *identity* of $i$, and builds a histogram. This requires $\Theta(N^{1/2})$ to $\Theta(N)$ samples depending on the property.

The quantum approach separates two tasks:
1. **Classical sampling** determines *which* elements to focus on (a set $A$ or a list $S$).
2. **[[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Quantum amplitude estimation]]** estimates $p_A = \sum_{i \in A} p_i$ using $O(\sqrt{1/p_A})$ queries instead of $O(1/p_A)$ classically.

The quantum oracle $\hat{O}_p$ maps $|s\rangle|0\rangle \to |s\rangle|O_p(s)\rangle$, and a phase-flip oracle $W_A$ marks elements in $A$. The `EstProb` subroutine (Theorem 5 of BHMT02) estimates $p_A$ with precision $\delta$ and error $\omega$ using $M \geq c\sqrt{p_A}/(\omega\delta)$ queries.

For the statistical difference algorithm, sampling $O(1)$ elements from the mixture $r = (p+q)/2$ and estimating their individual probabilities under both $p$ and $q$ gives enough information to estimate $\|p-q\|_1$. Each probability $p_i \sim 1/N$ for a "good" element, so each estimation costs $O(\sqrt{N})$ queries — giving $O(\sqrt{N})$ total.

## When to reach for it
Any time you want to test a property of an unknown distribution and the property can be expressed in terms of sums of probabilities $p_A$ for randomly chosen subsets $A$. The approach works for:
- $L_1$ distance estimation
- Uniformity testing
- Orthogonality / support disjointness
- Entropy estimation (mentioned in the paper)
- Fidelity $\sum_i \sqrt{p_i q_i}$ estimation

## Complexity
For estimating individual $p_i$ of an element sampled from $p$: $O(\sqrt{N})$ queries (since $p_i \geq \Omega(1/N)$ for "good" elements). For estimating $p_A$ for a set of size $M$: $O(\sqrt{N/M})$ queries.

## Caveat
Requires coherent access to the distribution oracle — not just samples. The oracle $\hat{O}_p$ must accept superposition inputs. This is a stronger model than the classical black-box sampling model, though Lemma 9 shows the two are equivalent classically. Also, estimating individual probabilities $p_i$ requires knowing *which* element $i$ to query about, so the classical sampling step is essential for selecting the right elements.

## Related notes
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Sample-then-Estimate Balancing for Query Optimisation]]
