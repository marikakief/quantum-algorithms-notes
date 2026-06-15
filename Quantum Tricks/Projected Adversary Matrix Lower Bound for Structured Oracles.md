# Projected Adversary Matrix Lower Bound for Structured Oracles

> **Source:** Kaplan, arXiv:1410.1434
> **Tags:** #trick #query-complexity #adversary-method #lower-bounds #oracle-reductions

## What it does

Proves a quantum query lower bound for a structured oracle problem by projecting each structured input to a known hard subproblem and lifting the subproblem's adversary matrix.

## The trick

Let $A$ be a structured oracle problem whose inputs contain more information than a hard problem $B$. A direct reduction from $B$ to $A$ may fail because an algorithm for $A$ can query parts of the structured oracle that have no counterpart in $B$.

Kaplan's double-encryption lower bound handles this by lifting the adversary matrix instead of reducing algorithms.

For an input $u$ of $A$, define a projection $\pi(u)$ that is an input to $B$. If $\Gamma_B$ is an adversary matrix for $B$, set

$$
\Gamma_A[u,v]=\Gamma_B[\pi(u),\pi(v)].
$$

When all projection fibres have the same size $D$, this often has tensor structure

$$
\Gamma_A=\Gamma_B\otimes J_{D\times D}.
$$

The numerator scales as

$$
\|\Gamma_A\|=D\|\Gamma_B\|.
$$

The proof task is then to show that every extra query available in $A$ can be matched, by a symmetry or row/column permutation, to a projected query from $B$. If so,

$$
\max_q \|\Gamma_A\circ\Delta_q\|=D\max_{\tilde q}\|\Gamma_B\circ\Delta_{\tilde q}\|,
$$

and the adversary ratio is inherited from $B$:

$$
\frac{\|\Gamma_A\|}{\max_q\|\Gamma_A\circ\Delta_q\|}
=
\frac{\|\Gamma_B\|}{\max_{\tilde q}\|\Gamma_B\circ\Delta_{\tilde q}\|}.
$$

In [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]], $A$ is double-encryption key extraction and $B$ is claw finding. The projection keeps only the two columns

$$
F_k(P),\qquad F_k^{-1}(C),
$$

while ignoring all other permutation values.

## When to reach for it

- The target oracle has extra query locations that make a black-box reduction awkward.
- The hard subproblem is visible as a projection or restriction of each target input.
- The target input distribution has enough symmetry to argue that off-projection queries are no more useful than projected ones.
- You need a lower bound, not just an upper-bound reduction.

## Complexity

The cost is proof work, not algorithmic cost. If the symmetry argument works, the lower bound from $B$ transfers to $A$ with the same asymptotic query exponent.

For Kaplan's application, the claw-finding lower bound gives

$$
Q(\mathrm{KE}_2)=\Omega(N^{2/3}).
$$

## Caveat

The fibre-size and symmetry conditions matter. If extra query locations reveal correlated information that cannot be permuted back to the projected query set, the lifted adversary matrix may give a weak bound or none at all.

This is also a query lower-bound technique; it does not by itself control circuit depth, QRAM, or fault-tolerant overhead.

## Related notes

- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]
- [[Quantum Meet-in-the-Middle as Claw Finding]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
