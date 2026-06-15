# Potts Partition Function via Cocycle-Code Weight Enumerators

> **Source:** Geraci-Lidar, arXiv:quant-ph/0703023
> **Tags:** #trick #Potts-model #coding-theory #Tutte-polynomial #partition-function

## What it does

Converts a uniform-coupling Potts partition function on a graph into a weight-enumerator evaluation for the graph's cocycle code.

## The trick

Given a graph $\Gamma=(E,V)$, form a cycle matroid matrix representation

$$
\mathrm{CMM}(\Gamma)=[I_{|V|-c(\Gamma)}\mid X].
$$

The row space of this matrix over $\mathbb F_q$ is the cocycle code $C(\Gamma)$ of length $n=|E|$ and dimension $|V|-c(\Gamma)$. Let

$$
A(x,y)=\sum_{i=0}^{n} A_i x^{n-i}y^i
$$

be its weight enumerator. Barg's relation gives, for prime $q$ and uniform Potts coupling,

$$
Z_\Gamma(y)=y^{-n}q^{c(\Gamma)}A(1,y),
$$

where

$$
Z_\Gamma(y)=\sum_\sigma y^{-|U(\sigma)|}.
$$

So evaluating $Z_\Gamma$ reduces to obtaining the Hamming-weight spectrum of a linear code attached to the graph.

## When to reach for it

Use this when a graph problem can be tied to a linear code whose weight enumerator is easier to access than the graph partition function directly. It is especially natural for Potts/Tutte problems, since both the Tutte polynomial and weight enumerators are matroidal invariants.

## Complexity

The graph-to-code step is polynomial: incidence matrix construction plus row reduction. The real cost is computing $A(1,y)$. In [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]], this becomes efficient only because the dual code is required to be irreducible cyclic and its weights are computed via [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|Gauss-sum phase estimation]].

## Caveat

This is a reduction, not an algorithm by itself. Weight enumerators of general linear codes are hard; the trick pays only when the attached code lands in a tractable family or has exploitable symmetry.

## Related notes

- [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]]
- [[Exact Weight Recovery by Divisibility-Gap Rounding]]
- [[Discrete-Log Recognition of Irreducible Cyclic Graph Codes]]
- [[Cyclotomic-Coset Compression for Cyclic-Code Weight Spectra]]
