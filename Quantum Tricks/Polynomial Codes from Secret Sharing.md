# Polynomial Codes from Secret Sharing

> **Source:** Aharonov and Ben-Or, arXiv:quant-ph/9906129
> **Tags:** #trick #quantum-code #CSS #secret-sharing #polynomial #algebraic

## What it does
Constructs quantum error-correcting codes by quantising classical secret sharing schemes based on polynomial evaluation. The resulting codes have algebraic structure that makes fault-tolerant gate implementation systematic.

## The trick
Start with the Ben-Or-Goldwasser-Wigderson classical secret sharing scheme over $\mathbb{F}_p$:
- Choose a random polynomial $f$ of degree $d$ over $\mathbb{F}_p$
- The secret is $f(0)$
- Party $i$ gets $f(x_i)$ for distinct points $x_1, \ldots, x_m \in \mathbb{F}_p$
- Any $d+1$ parties can reconstruct $f$ (and the secret) via interpolation
- Any $d$ parties learn nothing about the secret

To get a quantum code, replace "random polynomial" with "uniform superposition over all polynomials with a given value at 0":

$$|a\rangle_{\text{logical}} = \frac{1}{\sqrt{p^d}} \sum_{\substack{f: \deg f \leq d \\ f(0) = a}} |f(x_1)\rangle |f(x_2)\rangle \cdots |f(x_m)\rangle$$

This is a special case of a CSS code over $\mathbb{F}_p$, where:
- $C_1$ = evaluation code of degree-$d$ polynomials (corrects $\lfloor(m-d-1)/2\rfloor$ errors)
- $C_2^\perp$ = evaluation code of degree-$(m-d)$ polynomials (corrects $\lfloor d/2\rfloor$ errors)

The code corrects $\min\{\lfloor(m-d-1)/2\rfloor, \lfloor d/2\rfloor\}$ errors.

The key advantage: **all gates in $G_2$ can be applied pit-wise** (to corresponding physical qupits independently). When a gate (like multiplication) doubles the polynomial degree from $d$ to $2d$, a **degree reduction** step restores the code parameters using quantum polynomial interpolation — the quantum analogue of degree reduction in the BGW secret sharing protocol.

## When to reach for it
- When building fault-tolerant quantum computation over qupits ($p > 2$ states) and want a systematic gate implementation
- When the algebraic structure of the code matters for simplifying procedures — polynomial codes give a uniform framework where all fault-tolerant procedures follow the same pattern (apply pit-wise, reduce degree)
- When drawing on classical MPC/secret sharing results for quantum code construction

## Complexity
Block size $m$ qupits for the code, with correction capacity depending on $d$ and $m$. The degree reduction step requires $O(m)$ operations (interpolation). The systematic structure means procedure design is straightforward, unlike the ad-hoc constructions needed for some CSS codes.

## Caveat
- Works over $\mathbb{F}_p$ for prime $p$, so requires qupits rather than qubits. For $p = 2$, polynomial codes reduce to standard binary CSS codes.
- The threshold achieved ($\sim 10^{-6}$) is lower than architectures optimised for binary qubits, partly because qupits are harder to implement physically.
- The universality proof for $G_2$ (Theorem 9) requires $p > 3$. For $p = 2, 3$, the argument doesn't directly apply.
- Polynomial codes are used in the theoretical proof but are not the codes used in practice. Modern fault-tolerant architectures typically use surface codes or colour codes.

## Related notes
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
- [[Recursive Concatenation for Fault Tolerance]]
- [[Sparse Fault Path Analysis]]
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
