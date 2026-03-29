# Generalized Temperley-Lieb Algebra for Arbitrary Graphs

> **Source:** Aharonov, Arad, Eban & Landau, arXiv:quant-ph/0702008
> **Tags:** #trick #Temperley-Lieb #algebra #Tutte-polynomial #graph-algorithms

## What it does

Replaces the standard Temperley-Lieb algebra $TL_n(d)$ (fixed $n$ strands) with an infinite-dimensional algebra $\text{GTL}(d)$ that allows variable strand count. This means the Kauffman bracket of any planar graph can be computed via any representation of $\text{GTL}(d)$ — no Markov property needed.

## The trick

In the [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL algorithm]], a braid with $n$ strands maps to $TL_n(d)$, and the Jones polynomial equals the Markov trace of the corresponding TL element. The Markov trace is a special weighted trace satisfying $\text{tr}(XE_{n-1}) = \frac{1}{d}\text{tr}(X)$. Not every representation supports such a trace, so you're locked into representations with this property.

$\text{GTL}(d)$ sidesteps this entirely. Its elements are equivalence classes of planar diagrams connecting $n$ lower pegs to $m$ upper pegs (with $n, m$ variable), where two diagrams are equivalent if they differ only by adding straight strands on the right. The product rule: stack diagrams vertically, padding with straight strands to match peg counts.

For any planar graph $G$, the medial graph $L_G$ maps to a $\text{GTL}(d)$ element $\Psi_d(L_G)$ with zero in-pegs and zero out-pegs. Opening all crossings according to the Kauffman bracket rule yields only closed loops, so:

$$\Psi_d(L_G) = \langle L_G \rangle(d, u) \cdot \mathcal{I}$$

The Kauffman bracket is just the **scalar coefficient of the identity**. For any representation $\rho$:

$$\rho(\Psi_d(L_G)) = \langle L_G \rangle(d, u) \cdot \mathbf{1}$$

No Markov property, no trace — just the identity coefficient. This works because the identity element $\mathcal{I}$ maps to $\mathbf{1}$ in any representation.

## When to reach for it

- Extending braid-based quantum algorithms (Jones polynomial, etc.) to arbitrary planar combinatorial settings
- Any problem where a planar graph polynomial factors through a TL-like algebra but the input isn't naturally a braid
- When you want to use a convenient representation without verifying the Markov property

## Complexity

No overhead beyond the original algorithm. The medial graph has $|E|$ crossings, each contributing one elementary tangle. Total circuit depth: $O(|E|)$.

## Caveat

Still restricted to planar graphs. The $\text{GTL}(d)$ structure requires planarity (non-crossing diagrams). For non-planar graphs, you'd need a different algebraic framework (e.g., BMW algebra).

## Related notes
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Graph-to-TL-Product Mapping for Tutte Polynomial]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[Markov Trace Uniqueness for Jones Polynomial Computation]]
