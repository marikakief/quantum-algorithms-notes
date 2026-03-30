# Nested Grover Search for Bounded-Depth Predicates

> **Source:** Buhrman, Cleve, Wigderson, arXiv:quant-ph/9802040
> **Tags:** #trick #query-complexity #grover #polynomial-hierarchy #nested-search

## What it does
Achieves a near-quadratic quantum speedup for evaluating any bounded-depth predicate ($\Sigma_d$ or $\Pi_d$) over a black-box function.

## The trick

For a $\Sigma_d$ predicate on $n$ variables with $d$ alternating quantifiers:
$$\Sigma_d(f) = \bigvee_{x^{(1)}} \bigwedge_{x^{(2)}} \cdots Q_{x^{(d)}} f(x^{(1)}, \ldots, x^{(d)})$$

Build the evaluation bottom-up through $d$ layers:

1. **Layer $d$:** Use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] (or its complement for AND) to compute $g^{(d)}$ with $O(k_d \sqrt{2^{m_d}})$ queries to $f$, producing an [[Approximate Oracle via Compute-CNOT-Uncompute|approximate oracle]] for $g^{(d)}$.

2. **Layer $d-1$:** Use Grover on $g^{(d)}$ to compute $g^{(d-1)}$, with $O(k_{d-1} \sqrt{2^{m_{d-1}}})$ calls to the approximate $g^{(d)}$ oracle.

3. Continue up to layer 1, which gives the final answer.

Setting all inner precisions $k_2, \ldots, k_d = O(n)$ and $k_1 = O(1)$:

$$T(\Sigma_d) \in O\left(\sqrt{2^n} \cdot n^{d-1}\right)$$

The $n^{d-1}$ overhead comes from the precision amplification needed at each nesting level to keep accumulated errors bounded across the $\sqrt{2^n}$ total oracle calls.

## When to reach for it

- Evaluating quantified Boolean formulas with a fixed number of alternations.
- Any composite function with a tree-like AND/OR structure over a base oracle.
- Quantum speedups for problems in the polynomial hierarchy.

## Complexity

$O(\sqrt{N} \cdot (\log N)^{d-1})$ queries for depth-$d$ predicates on $N$-bit inputs. For any fixed $d$, this is a near-quadratic speedup over the classical $\Theta(N)$.

If willing to accept slightly worse than quadratic speedup ($O(N^{1/2+\delta})$), the error probability can be made doubly exponentially small.

## Caveat

The $(\log N)^{d-1}$ factor grows with depth, so this isn't useful for unbounded depth. For PARITY ($d = n$) and MAJORITY, the approach fails entirely — these require $\Omega(N)$ queries, ruling out any polynomial speedup. The trick only helps for the "easy" end of the hierarchy.

## Related notes
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]]
- [[Approximate Oracle via Compute-CNOT-Uncompute]]
- [[Black-Box to Communication Reduction]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — later achieves optimal $O(\sqrt{N})$ for arbitrary AND-OR formulas via quantum walk
