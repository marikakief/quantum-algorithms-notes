# Markov Trace Uniqueness for Jones Polynomial Computation

> **Source:** Aharonov, Jones & Landau, arXiv:quant-ph/0511096; originally due to Jones (1985)
> **Tags:** #trick #jones-polynomial #Temperley-Lieb #uniqueness

## What it does

Guarantees that any representation of the Temperley-Lieb algebra that supports a trace satisfying three simple properties computes the Jones polynomial correctly — no matter how different the representation looks from the "standard" one.

## The trick

The Markov trace $\text{tr}: TL_n(d) \to \mathbb{C}$ is uniquely determined by:

1. $\text{tr}(1) = 1$
2. $\text{tr}(XY) = \text{tr}(YX)$ (cyclicity)
3. $\text{tr}(XE_{n-1}) = \frac{1}{d}\text{tr}(X)$ for $X \in TL_{n-1}(d)$ (Markov property)

**Proof of uniqueness:** Every reduced word in $TL_n(d)$ contains at most one $E_{n-1}$. By the Markov property, $\text{tr}$ on $TL_n$ reduces to $\text{tr}$ on $TL_{n-1}$. By induction down to $TL_1 = \{I\}$, the trace is determined.

This means: to compute the Jones polynomial, find *any convenient* representation of $TL_n(d)$, identify the trace with the Markov property in that representation, and estimate it. The path model representation is convenient for quantum computing — it's unitary and has a nice basis — but any representation would give the same answer.

## When to reach for it

- Designing quantum algorithms for knot invariants or other quantities defined by traces on algebras.
- When you want to compute the "right" quantity but in a non-standard representation.
- Algebraic proofs that different-looking algorithms compute the same thing.

## Complexity

No computational overhead — this is a mathematical guarantee, not an algorithm. It's what makes the AJL algorithm *correct*, not what makes it *efficient*.

## Caveat

The uniqueness only holds for *representations* of $TL_n(d)$ — if your operators don't actually satisfy the TL relations, you're computing something else. Also, the trace must be well-defined on the image of the representation, which can fail if the representation has a kernel.

## Related notes
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[Hadamard Test for Trace Estimation]]
