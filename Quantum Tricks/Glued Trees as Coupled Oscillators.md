
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, arXiv:2303.13012; original glued trees problem from Childs, Cleve, Deotto, Farhi, Gutmann, Spielman (2003)
> **Tags:** #trick #oracle-separation #glued-trees #quantum-walk #coupled-oscillators #classical-lower-bound

## What it does
Reduces the glued trees problem (an oracle problem with known exponential quantum-vs-classical separation) to a coupled oscillator simulation, proving that classical algorithms need $2^{\Omega(n)}$ queries to estimate kinetic energy in general oscillator networks.

## The trick

Take the glued trees graph: two balanced binary trees of depth $n$ with their leaves randomly connected, giving $N = 2^{n+1} - 2$ vertices. Place a unit mass at each vertex and a unit spring at each edge (plus wall springs at both roots).

Set initial conditions: ENTRANCE root at velocity 1, everything else at rest.

The symmetry of the binary tree structure allows dimensional reduction from $N$ oscillators to $2n$ oscillators along the "column coordinate" $z_l(t)$, where each column aggregates all masses at the same depth. The reduced dynamics is governed by a $2n \times 2n$ tridiagonal matrix $\tilde{A}$ with coupling $\sqrt{2}$ between most columns and coupling $2$ at the gluing point.

**Key lemma:** The time-averaged kinetic energy of the EXIT root satisfies $P_{\text{EXIT}}(T) \geq 1/(4n) - O(1/\exp(n))$ for $T = O(n^4)$.

**Proof sketch:** The eigenvectors of $\tilde{A}$ have a reflection symmetry ($|\langle \lambda_l | 1\rangle| = |\langle \lambda_l | 2n\rangle|$ for all $l$), so the time-averaged transfer from ENTRANCE to EXIT is $\geq 1/(4n)$ by Cauchy-Schwarz. The spectral gap $\Delta = \Omega(1/n^3)$ ensures this average is reached by $T = O(n^4)$. (Numerically, $T = O(n)$ suffices.)

Since the original glued trees problem requires $2^{\Omega(n)}$ classical queries, and it reduces to kinetic energy estimation, the classical lower bound transfers.

## When to reach for it

- Proving classical hardness of dynamical simulation problems by reduction from known oracle separations.
- Showing that a physical simulation problem can encode graph-traversal problems.
- Establishing that a quantum algorithm's advantage is not an artifact of the problem formulation.

## Complexity

Quantum: $O(\text{poly}(n))$ time for the simulation + $O(\text{poly}(n))$ repetitions for kinetic energy estimation, since the EXIT kinetic energy fraction is $\Omega(1/\text{poly}(n))$.

Classical: $2^{\Omega(n)}$ queries (inherited from Childs et al. 2003).

## Caveat

The glued trees graph is not geometrically local — it's a random bipartite matching between the leaf layers. This limits the "naturalness" of the lower bound. For geometrically local oscillator networks, the exponential separation may not hold. The paper does not claim hardness for physically typical oscillator systems; the claim is about the general (worst-case) problem.

## Related notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]
- [[Feynman-Kitaev Construction with Non-Negative Entries]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
