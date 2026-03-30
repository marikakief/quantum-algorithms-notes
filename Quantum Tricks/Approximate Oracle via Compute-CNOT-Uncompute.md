# Approximate Oracle via Compute-CNOT-Uncompute

> **Source:** Buhrman, Cleve, Wigderson, arXiv:quant-ph/9802040
> **Tags:** #trick #query-complexity #oracle-construction #amplitude-amplification

## What it does
Builds an approximate quantum oracle for a derived Boolean function $g$ (defined in terms of a base oracle $f$) that can be used as a subroutine in further quantum algorithms.

## The trick

Suppose you have a quantum procedure $G$ that computes $g(x)$ into the first qubit with success probability $\geq 1 - 2^{-k}$, using $O(k\sqrt{M})$ calls to $f$ (e.g., via repeated [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] with $M$ inputs).

After applying $G$ to $|x\rangle|0\ldots0\rangle$, the state is:
$$\alpha|g(x)\rangle|A\rangle + \beta|\overline{g(x)}\rangle|B\rangle$$
where $|\alpha|^2 \geq 1 - 2^{-k}$.

To build the oracle $V \approx U_g$, act on $|z\rangle|x\rangle|0\ldots0\rangle$:

1. Apply $G$ (computes $g$ approximately)
2. CNOT from the "answer" qubit onto the target qubit $|z\rangle$
3. Apply $G^\dagger$ (uncomputes the workspace)

The result is $|z \oplus g(x)\rangle|x\rangle|0\ldots0\rangle$ plus an error term of norm $\leq \sqrt{2} \cdot 2^{-k/2}$ per basis state. For a general superposition over $\sqrt{D}$ basis states, the total error is $\leq \sqrt{D} \cdot \sqrt{2} \cdot 2^{-k/2}$.

Setting $k = O(n)$ makes the per-use error small enough to compose $O(\sqrt{D})$ approximate oracle calls without the accumulated error exceeding a constant.

## When to reach for it

- Nested quantum search: the inner search result becomes an oracle for the outer search.
- Any hierarchical quantum algorithm where a subroutine's output is needed as a coherent oracle.
- Building oracles for composed functions $F \circ G$ in query complexity.

## Complexity

$O(k\sqrt{M})$ queries to $f$ per approximate oracle call, where $k$ controls the precision and $M$ is the domain size of $g$'s inner computation.

## Caveat

The error scales with $\sqrt{D}$ (the dimension of the input superposition), so you need $k$ to grow logarithmically in $D$ to keep the total error bounded. For deeply nested compositions, this adds polylogarithmic overhead at each level.

## Related notes
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]]
- [[Nested Grover Search for Bounded-Depth Predicates]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
