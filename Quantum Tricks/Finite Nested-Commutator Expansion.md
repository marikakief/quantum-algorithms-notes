
> **Tags:** #trick #trotter #commutators
> **Source:** Childs, Su, Tran, Wiebe, Zhu, arXiv:1912.08854

## What it does

Expresses the leading error of a $p$-th order [[Order-Condition Cancellation in Product Formulas|product formula]] as a finite sum of $(p+1)$-fold nested commutators plus a controlled integral remainder, without truncating an infinite BCH series.

Concretely: expand the conjugations $e^{A}Be^{-A}$ layer by layer to order $p$, collect commutator terms, and handle the remainder with an integral bound. The output is a representation of the form
$$
S(t) - e^{tH} = \sum_{\text{finite}} c_{\gamma_1\cdots\gamma_{p+1}} [H_{\gamma_{p+1}},\cdots,[H_{\gamma_2},H_{\gamma_1}]\cdots] \cdot t^{p+1} + R(t),
$$
where $R(t) = O(t^{p+2})$ and the finite sum has an explicit bounded coefficient.

## Why this is better than BCH truncation

BCH gives a formal infinite series of nested commutators. Truncating introduces an uncontrolled tail that requires locality or closure of the Lie algebra to bound. The finite nested-commutator expansion sidesteps this: the remainder is controlled directly from the integral form, not from a tail bound on an infinite series.

## When to reach for it

- High-order [[Trotter Commutator-Scaling Bound|Trotter]] proofs where BCH reasoning would introduce uncontrolled remainder.
- Need accurate order accounting at specific order $p$.
- Bounding $\tilde{\alpha}_{\mathrm{comm}}$ for a concrete Hamiltonian.

## Caveat

Symbolic bookkeeping scales quickly with order — for $p=4$ or $p=6$ the number of commutator terms is large. Automated computer algebra useful for higher orders.

## Related Paper Notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
