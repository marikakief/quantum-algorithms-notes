# Slope-Uncomputed In-Place Elliptic-Curve Shifts

> **Source:** Proos and Zalka, arXiv:quant-ph/0301141
> **Tags:** #trick #elliptic-curves #reversible-computation #quantum-arithmetic

## What it does

Implements a fixed elliptic-curve point shift $S\mapsto S+A$ in place by computing the slope and then uncomputing it from the output coordinates.

## The trick

Let $S=(x,y)$ and let the fixed shift point be $A=(\alpha,\beta)$. In the generic affine case,

$$
\lambda=\frac{y-\beta}{x-\alpha},
\qquad
x'=\lambda^2-(x+\alpha),
\qquad
 y'=\lambda(x-x')-y.
$$

The key identity is that the same slope can be recovered from the output side:

$$
\lambda=\frac{-y'+\beta}{x'-\alpha}.
$$

So the reversible update can be organised as

$$
(x,y)
\leftrightarrow
(x,\lambda)
\leftrightarrow
(x',\lambda)
\leftrightarrow
(x',y').
$$

This avoids the generic reversible pattern

$$
|S,0\rangle\mapsto |S,S+A\rangle\mapsto |0,S+A\rangle,
$$

which would need a second point register and a full uncomputation of $S$.

## When to reach for it

- Reversible implementations of affine elliptic-curve addition by a fixed point.
- Group-action algorithms where the operation is a shift by a known classical group element.
- Circuits where work qubits, rather than arithmetic count, dominate the design.

## Complexity

One fixed group shift uses two divisions of the form

$$
(x,y)\leftrightarrow (x,y/x),
$$

one modular multiplication for $\lambda^2$, and several modular additions/subtractions. In Proos--Zalka, each division uses two reversible extended Euclidean runs and two modular multiplications.

## Caveat

This is the generic affine formula. It needs separate handling, or a controlled approximation argument, for $S=A$, $S=-A$, and $S=O$. Proos--Zalka pair it with [[Random-Offset Generic Elliptic-Curve Group Shifts]].

## Related notes

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]
- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Random-Offset Generic Elliptic-Curve Group Shifts]]
- [[Fixed-Round Reversible Montgomery-Kaliski Inversion]]
- [[Desynchronised Reversible Euclidean Algorithm]]
