
> **Source:** Andrew M. Childs, Yuan Su, Minh C. Tran, Nathan Wiebe, and Shuchen Zhu, *A Theory of Trotter Error*, arXiv:1912.08854, Phys. Rev. X **11**, 011020 (2021)  
> **Links:** [arXiv](https://arxiv.org/abs/1912.08854) · [PRX](https://doi.org/10.1103/PhysRevX.11.011020)  
> **Tags:** #trick #trotter #commutators #hamiltonian-simulation

## Idea

For a $p$-th order product formula with anti-Hermitian summands, define the $(p+1)$-fold nested-commutator norm:
$$
\tilde{\alpha}_{\mathrm{comm}} = \sum_{\gamma_1,\ldots,\gamma_{p+1}} \bigl\| [H_{\gamma_{p+1}}, [H_{\gamma_p}, \cdots, [H_{\gamma_2}, H_{\gamma_1}]\cdots]] \bigr\|.
$$
Then the [[Order-Condition Cancellation in Product Formulas|product-formula error]] satisfies $\|S(t) - e^{tH}\| = O(\tilde{\alpha}_{\mathrm{comm}} \cdot t^{p+1})$.

This is a **finite** representation — no BCH tail, no locality assumption needed. The bound captures exactly how near-commutativity reduces error: if terms nearly commute, many $(p+1)$-fold commutators are small, and the bound is much tighter than $\|H\|^{p+1} t^{p+1}$.

## Why it matters

This is what explains the gap between worst-case Trotter bounds (which can be enormous) and observed performance on local or structured Hamiltonians. For a locally interacting spin chain, $\tilde{\alpha}_{\mathrm{comm}}$ scales geometrically rather than with the full operator norm. Proving that product formulas are competitive with [[Qubitization Iterate|qubitization]]-based methods in physical regimes requires something like this bound.

## When to use it

- Hamiltonian has locality or sparse interaction structure.
- Naïve Trotter bounds are embarrassingly pessimistic.
- Comparing product formulas fairly against [[Block-Encoding Composition Algebra|block-encoding]]-based methods.

## What to remember

The right question is not "how big are the terms?" but "which $(p+1)$-fold commutators actually survive?" For $p=1$: second-order commutators. For $p=2$: third-order nested commutators. Each step up in order shrinks the commutator sum further for well-structured systems. See [[Finite Nested-Commutator Expansion]] for the explicit representation.

## Related notes

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Finite Nested-Commutator Expansion]]
- [[Interaction-Picture Split for Product Formulas]]
