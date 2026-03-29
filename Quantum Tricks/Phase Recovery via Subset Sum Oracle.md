# Phase Recovery via Subset Sum Oracle

> **Source:** Regev, arXiv:cs/0304005
> **Tags:** #trick #phase-estimation #hidden-subgroup-problem #dihedral-group

## What it does
Extracts an unknown phase from a tensor product of coset states by reducing the identification of interfering basis states to an average-case subset sum problem.

## The trick
Given $r \approx \log N$ registers, each in state $\frac{1}{\sqrt{2}}(|0\rangle + e^{2\pi i a_k d/N}|1\rangle)$ (obtained by QFT + measurement on dihedral coset states), the combined state is:

$$\sum_{\bar{\alpha} \in \{0,1\}^r} e^{2\pi i d \cdot t_{\bar{\alpha}}/N} |\bar{\alpha}\rangle \quad \text{where } t_{\bar{\alpha}} = \sum_{k} \alpha_k a_k$$

Compute $\lfloor t_{\bar{\alpha}}/2 \rfloor$ and measure. The state collapses to a superposition of two strings $\bar{\alpha}_1, \bar{\alpha}_2$ whose sums differ by 1: $t_{\bar{\alpha}_2} - t_{\bar{\alpha}_1} = 1$.

The subset sum oracle provides the reverse mapping: given the measured value $s = \lfloor t/2 \rfloor$, call the oracle on $2s$ and $2s+1$ to recover both $\bar{\alpha}_1$ and $\bar{\alpha}_2$. Apply a unitary mapping $|\bar{\alpha}_1\rangle \to |0\rangle$, $|\bar{\alpha}_2\rangle \to |1\rangle$. The phase difference $e^{2\pi i d/N}$ now sits on a single qubit, measurable by Hadamard + measurement.

Repeat with matchings of difference $q = 2, 4, 8, \ldots$ to build up exponential precision on $d$, analogous to [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's iterative phase estimation]].

## When to reach for it
When phase information is distributed across multiple qubits in a product structure, and you have an oracle that can identify which computational basis states are superposed. The key insight: the QFT on coset states produces random inputs to subset sum, turning a number-theoretic oracle into a quantum measurement primitive.

## Complexity
$O(\log^{O(1)} N)$ calls to the subset sum oracle per phase bit. Total: $O(\log N \cdot \log^{O(1)} N)$ oracle calls for full recovery of $d$.

## Caveat
Requires a subset sum oracle that works on a $1/\text{poly}(\log N)$ fraction of random instances with density $\approx 1$. No efficient such oracle is known — this is the conditional part of Regev's result.

## Related notes
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Coset Sampling via Fourier Transform]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Lattice Point Spacing for Pairwise Isolation]]
