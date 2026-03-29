# Operator Grouping for Approximation Window Tightening

> **Source:** Aharonov, Arad, Eban & Landau, arXiv:quant-ph/0702008 (Section 10)
> **Tags:** #trick #approximation #non-unitary #BQP-complete

## What it does

Reduces the additive approximation window of a quantum algorithm that applies a sequence of non-unitary operators, by grouping consecutive operators and applying their classical product as a single operator.

## The trick

When a quantum algorithm applies operators $M_1, M_2, \ldots, M_N$ in sequence using [[Non-Unitary Gate Implementation via Post-Selection|post-selection]], the approximation scale is $\prod_{i=1}^N \|M_i\|$. But the actual quantity of interest has magnitude $\|\prod_i M_i\|$, which can be much smaller (since $\|\prod M_i\| \leq \prod \|M_i\|$).

Partition the sequence into groups $S_j = M_{i_j} \cdots M_{i_{j+1}-1}$. Compute each product $\rho(S_j)$ classically (feasible if each group acts on $O(\log n)$ qubits), then apply $\rho(S_j)$ as a single operator. The new scale is:

$$\Delta_{\text{grp}} = \prod_j \|\rho(S_j)\|$$

If the group products are approximately unitary (norm $\approx 1$), this can be exponentially smaller than the ungrouped scale.

**Natural application:** In the universality proof, $\text{polylog}(1/\varepsilon)$ crossing operators approximate one two-qubit gate. Their product is approximately unitary on the relevant subspace $K = H_{8,1\to 1}$. Grouping these crossings together gives $\|\rho(S_j)\| \approx 1$, matching $\Delta_{\text{alg}}$ to $\Delta_{\text{hard}}$.

**Subtlety:** The product might be near-unitary on the relevant subspace $K$ but have large norm on other subspaces of $H_8^*$. For BQP-completeness, you need simultaneous density and efficiency across all subspaces, requiring the generalised [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev theorem]] for direct sums of spaces.

## When to reach for it

- Any quantum algorithm applying a long sequence of non-unitary operators where consecutive operators have a natural grouping (e.g., they approximate one logical gate)
- Proving BQP-completeness when the algorithmic scale is larger than the hardness scale
- Improving the precision of [[Non-Unitary Gate Implementation via Post-Selection|post-selected]] quantum computations

## Complexity

Classical precomputation: $O(\text{poly}(n))$ to compute each group product (the operators act on $O(\log n)$ qubits). No additional quantum cost — you're just applying fewer, better operators.

## Caveat

The problem formulation must be modified to include the grouping as part of the input. Without a grouping oracle, finding the optimal partition is an open problem. The modified problem (graph + partition) is less natural than the original (graph only).

Also, the trick helps only when the product-of-norms substantially exceeds the norm-of-product — i.e., when there's significant cancellation between consecutive operators. For unitary operators, there's nothing to gain (norms are already 1).

## Related notes
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[Non-Unitary Gate Implementation via Post-Selection]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
