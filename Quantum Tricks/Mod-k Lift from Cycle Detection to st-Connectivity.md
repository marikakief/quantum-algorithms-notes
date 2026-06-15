# Mod-k Lift from Cycle Detection to st-Connectivity

> **Source:** Cade--Montanaro--Belovs, arXiv:1610.00581
> **Tags:** #trick #graph-algorithms #st-connectivity #cycle-detection #span-programs

## What it does

Turns a cycle-detection question into an $s$-$t$ connectivity question by replacing each vertex $v$ by labelled copies $v_b$ and making edge traversal update $b$ modulo a small constant.

## The trick

Orient every edge of $G$. For $k=3$, build a lift graph with vertices
$$
\{s,t\}\cup\{v_b:v\in V,\ b\in\mathbb Z_3\}.
$$
For each oriented edge $(u,v)$ and each $b$, add
$$
(u_b,v_{b+1}).
$$
Add $(s,k_0)$ and $(t,k_1)$.

A cycle with orientation imbalance
$$
D=\#\text{clockwise}-\#\text{anticlockwise}
$$
connects different copies iff $D\not\equiv0\pmod3$. If $k$ lies on a good cycle of length $\ell$, then $s$ and $t$ are connected by a path of length $O(\ell)$.

For bipartiteness, use $k=2$: odd cycles connect the two copies, even cycles do not.

## When to use it

- Reducing graph-property witnesses to connectivity witnesses.
- Making cycle parity/modular structure visible to span programs or quantum walks.
- Designing space-efficient algorithms where the lifted graph can be queried implicitly.

## Caveat

For $k=3$, some cycles are invisible when $D\equiv0\pmod3$. The paper repairs this with [[Pairwise-Independent Edge-Flipping to Repair Modular Cycle Tests]].

## Related notes

- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
- [[Log-Space Span-Program st-Connectivity as a Subroutine]]
