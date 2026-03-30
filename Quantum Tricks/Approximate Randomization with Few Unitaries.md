# Approximate Randomization with Few Unitaries

> **Source:** Hayden, Leung, Shor & Winter, arXiv:quant-ph/0307104
> **Tags:** #trick #randomization #private-channel #approximate-encryption #quantum-shannon-theory

## What it does

Shows that $O(d \log d)$ randomly chosen unitaries suffice to approximately randomize any $d$-dimensional quantum state, halving the key length compared to perfect randomization ($\log d$ vs $2\log d$ bits).

## The trick

Choose $n = O(d\log d / \epsilon^2)$ unitaries $\{U_j\}$ i.i.d. from the Haar measure on $U(d)$. The map

$$R(\varphi) = \frac{1}{n}\sum_{j=1}^n U_j \varphi U_j^\dagger$$

is $\epsilon$-randomizing with high probability: $\|R(\varphi) - I/d\|_\infty \leq \epsilon/d$ for all states $\varphi$.

The proof combines:
1. **[[Large Deviation Bound for Random Unitaries|Concentration]]:** For a fixed $\varphi$ and projector $P$, the empirical average $\frac{1}{n}\sum_j \operatorname{Tr}(U_j \varphi U_j^\dagger P)$ concentrates around $p/d$ with deviation probability $\leq 2\exp(-Cnp\epsilon^2)$.
2. **$\epsilon$-net:** Cover the state space with $(5/\epsilon)^{2d}$ pure states. Union bound over all pairs of net points for $\varphi$ and the eigenstates of $R(\varphi) - I/d$.

Why does the factor of 2 disappear? Perfect randomization must erase *all* correlations (including quantum entanglement with other systems), which requires $d^2$ unitaries (the Pauli group). Approximate randomization only needs to erase *local* information — quantum correlations survive but become invisible to LOCC.

## When to reach for it

- Constructing approximate private quantum channels with minimal key.
- Building quantum data hiding schemes (combine with the [[LOCC Indistinguishability from Approximate Randomization|LOCC hiding]] observation).
- Proving existence of states with locked classical correlations.
- As a building block for [[Decoupling by Random Measurement|decoupling]] arguments (approximate randomization of a subsystem decouples it from the reference).
- Constructing approximate unitary $t$-designs (the randomization result shows random unitaries form approximate $1$-designs at suboptimal but sufficient rates).

## Complexity

Sampling: $O(d^2)$ parameters per Haar-random unitary. Applying the map: $n$ unitary applications, so $O(d^2 \cdot d\log d / \epsilon^2)$ total operations. Exponential in the number of qubits $\ell = \log d$.

For explicit (non-random) constructions, approximate $2$-designs achieve similar guarantees with $\mathrm{poly}(d)$ unitaries that can each be implemented in $\mathrm{poly}(\ell)$ gates.

## Caveat

The construction is probabilistic — a random choice of unitaries works with high probability but no deterministic construction matching $O(d\log d)$ is known. The operator norm criterion ($\|\cdot\|_\infty$) is stronger than trace norm ($\|\cdot\|_1$); the paper needs the stronger version for the data hiding application. The accessible information bound ($\chi \leq \epsilon/\ln 2$) is adequate for security but may be loose.

## Related notes
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]]
- [[LOCC Indistinguishability from Approximate Randomization]]
- [[Large Deviation Bound for Random Unitaries]]
- [[Decoupling by Random Measurement]]
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]]
