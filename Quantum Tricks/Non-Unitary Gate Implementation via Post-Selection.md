# Non-Unitary Gate Implementation via Post-Selection

> **Source:** Aharonov, Arad, Eban & Landau, arXiv:quant-ph/0702008
> **Tags:** #trick #non-unitary #post-selection #LCU

## What it does

Implements a non-unitary operator $M$ on a quantum computer using an ancilla qubit and post-selection, with success probability $1/\|M\|^2$.

## The trick

Given a non-unitary $M$ with $\|M\| > 1$, construct a $2 \times 2$ block unitary:

$$
U = \begin{pmatrix} M/\|M\| & \cdot \\ \cdot & \cdot \end{pmatrix}
$$

where the off-diagonal blocks are chosen so $U$ is unitary ($U$ always exists by the unitary dilation theorem).

Apply $U$ to the system plus one ancilla qubit initialised to $|0\rangle$. Post-select on the ancilla being $|0\rangle$. The effective operation on the system is $M/\|M\|$, and the success probability is $\|M|\psi\rangle\|^2 / \|M\|^2$.

For a sequence of non-unitary operators $M_1, \ldots, M_L$, the total success probability is at most $1/\prod_i \|M_i\|^2$, which can be exponentially small.

## When to reach for it

- Implementing non-unitary representations in quantum algorithms (e.g., Tutte polynomial at non-unitary parameters).
- Any quantum algorithm that needs to apply a sub-unitary or contractive operator.
- As a building block for [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]] — decompose $M$ as a linear combination of unitaries and implement probabilistically.

## Complexity

Success probability $1/\|M\|^2$ per non-unitary step. For $L$ steps: $1/\prod_{i=1}^L \|M_i\|^2$ total. This can be exponentially small, making the additive approximation window exponentially large.

## Caveat

The exponential blowup in the approximation window is the central limitation. For the Tutte polynomial, this means the algorithm's approximation quality depends on the product of operator norms across all graph edges — which can make the result trivial (approximation window larger than the answer) for some parameter ranges.

This is also why the Potts model at physical parameters remains open: the operator norms at real, positive Potts temperatures can make the approximation window too large to be useful.

## Related notes
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[Operator Grouping for Approximation Window Tightening]] — reduces the approximation window when applying many non-unitary operators in sequence
- [[Generalized Temperley-Lieb Algebra for Arbitrary Graphs]] — the algebraic setting where non-unitary representations arise
