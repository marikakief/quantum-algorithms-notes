# Collision-Limited Quantum Speedup for Truncated Differentials

> **Source:** Kaplan, Leurent, Leverrier, Naya-Plasencia, arXiv:1510.05836
> **Tags:** #trick #quantum-walks #element-distinctness #differential-cryptanalysis

## What it does

Explains why truncated differential attacks often get less than a quadratic quantum speedup: the bottleneck is pair/collision finding inside structures, not independent marked-item search.

## The trick

A truncated differential uses sets of differences $D_{\rm in}$ and $D_{\rm out}$ rather than a single pair $(\delta_{\rm in},\delta_{\rm out})$. Structures compress data: a set of $2^{\Delta_{\rm in}}$ plaintexts can generate many pairs with input difference in $D_{\rm in}$.

Classically, after generating structures, the attack looks for pairs $(x,y)$ satisfying

$$
x\oplus y\in D_{\rm in},\qquad E(x)\oplus E(y)\in D_{\rm out}.
$$

Quantumly, this is a collision/pair-search problem. Use [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis element distinctness]] rather than plain [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]]. For a list of size $N$ with one good pair, the query cost is $O(N^{2/3})$, not $O(N^{1/2})$.

Kaplan--Leurent--Leverrier--Naya-Plasencia derive for a truncated differential distinguisher:

$$
T_C^{\rm tr.dist}=\max\left\{2^{(h_T+1)/2},2^{h_T-\Delta_{\rm in}+1}\right\},
$$

but

$$
T_{Q2}^{\rm tr.dist}=\max\left\{2^{(h_T+1)/3},2^{(h_T+1)/2-\Delta_{\rm in}/3}\right\}.
$$

So the quantum exponent is not simply half the classical exponent.

## When to reach for it

- Classical attacks whose data advantage comes from structures, collisions, or many-pair generation.
- Quantum analyses where someone claims a blanket quadratic speedup for a truncated or boomerang-style attack.
- Any setting where the object being searched is an unordered pair drawn from a generated list.

## Complexity

The core pair-search cost is $O(N^{2/3}K^{-1/3})$ for $K$ good pairs in a random list of size $N$, matching the paper's Theorem 5. Additional Grover layers may appear when searching over structures.

## Caveat

This accounting assumes random-like placement of good pairs in structures. Highly structured pair distributions may need a more tailored quantum walk.

## Related notes

- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Nested Grover Checking for Cryptanalytic Key Recovery]]
