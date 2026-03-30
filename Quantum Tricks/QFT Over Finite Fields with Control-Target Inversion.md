# QFT Over Finite Fields with Control-Target Inversion

> **Source:** de Beaudrap, Cleve, Watrous, arXiv:quant-ph/0011065
> **Tags:** #trick #QFT #finite-fields #query-complexity #hidden-subgroup

## What it does
Defines a quantum Fourier transform over a finite field $\mathrm{GF}(p^n)$ that swaps control and target registers when conjugating a controlled-addition gate — generalising the well-known fact that $H \cdot \text{CNOT} \cdot H$ swaps control and target in the qubit case.

## The trick
Choose any nonzero linear map $\phi: \mathrm{GF}(q) \to \mathrm{GF}(p)$ and define:

$$F_{q,\phi}: |x\rangle \mapsto \frac{1}{\sqrt{q}} \sum_{y \in \mathrm{GF}(q)} \omega^{\phi(xy)} |y\rangle, \quad \omega = e^{2\pi i / p}$$

The multiplication $xy$ is in the *field* $\mathrm{GF}(q)$, not componentwise. This is what gives the transform its algebraic power.

**Control/target inversion:** For the controlled-ADD$_s$ gate ($|x\rangle|y\rangle \mapsto |x\rangle|y + sx\rangle$):

$$(F^\dagger \otimes F) \circ \text{ADD}_s \circ (F \otimes F^\dagger) = \text{reversed-ADD}_s$$

where reversed-ADD$_s$ maps $|x\rangle|y\rangle \mapsto |x + sy\rangle|y\rangle$.

**Efficient circuit:** There exists a matrix $M_\phi$ (an $n \times n$ Hankel matrix over $\mathrm{GF}(p)$) such that $\phi(xy) = \vec{x}^T M_\phi \vec{y}$, and the QFT decomposes as:

$$F_{q,\phi} = M_\phi^{-1} \cdot (F_p^{\otimes n})$$

i.e., $n$ independent small QFTs modulo $p$, followed by a linear change of basis. For $p = 2$: the small QFTs are just Hadamard gates, and the change of basis is $O(n^2)$ CNOTs. Total: $O(n^2)$ gates.

## When to reach for it
- Constructing quantum query separations over algebraic structures (finite fields, extension fields)
- Solving hidden structure problems where the structure involves field multiplication (not just addition)
- Building single-query exact quantum algorithms for problems parameterised by elements of $\mathrm{GF}(2^n)$
- Whenever you need a QFT that respects both additive *and* multiplicative structure of a finite field

## Complexity
$O(n^2 (\log p)^2) + n \cdot C(p, \varepsilon/n)$ gates, where $C(p,\varepsilon)$ is the cost of the QFT modulo $p$. For $p = 2$: exactly $O(n^2)$ gates with no approximation error.

## Caveat
Only works over *fields* (no zero divisors). For general rings like $\mathbb{Z}_{2^n}$, the multiplicative structure breaks down — zero divisors allow classical binary search strategies that destroy the exponential separation. The construction extends to matrix rings $\mathrm{GF}(p^n)^{m \times m}$ (via element-wise QFT + transpose), but classical lower bounds for matrix ring problems are not established.

For large primes $p$, the QFT modulo $p$ itself costs $O(p^2 \log p)$ gates exactly, which is exponential in $\log p$. Approximate versions cost $O(\log p \cdot \log\log p + \log p \cdot \log(1/\varepsilon))$.

## Related notes
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]
- [[Collision-Based Classical Lower Bound for Field-Structured Problems]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
