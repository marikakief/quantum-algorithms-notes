# Classical Reduction of Semigroup DLP to Cycle-Group DLP

> **Source:** Banin--Tsaban, arXiv:1310.7903
> **Tags:** #trick #semigroups #discrete-log #cryptanalysis

## What it does

Reduces one-generator discrete log in a periodic semigroup to ordinary DLP in the cyclic group formed by the eventual cycle of that generator.

## The trick

For a periodic semigroup element $g$, let $\ell,n$ be minimal with
$$
g^{\ell+n}=g^\ell,
$$
and let $t$ be minimal with $tn>\ell$. Then
$$
G=\{g^{\ell+k}:0\leq k<n\}
$$
is a cyclic group of order $n$, with identity
$$
e=g^{tn}
$$
and generator
$$
r=g^{tn+1}.
$$
The semigroup tail has no inverses, but once an element has reached $G$, ordinary group DLP applies.

To solve $h=g^x$:

1. Recover $n$ and $t$.
2. Test whether $h\in G$ via
   $$
   h\in G\quad\Longleftrightarrow\quad g^n h=h.
   $$
3. If $h\in G$, call a group-DLP oracle to find $x'$ with $r^{x'}=h$ and return
   $$
   k=x'(tn+1).
   $$
4. If $h\notin G$, multiply by enough $n$-steps to move it into $G$, solve the group DLP there, then subtract the known $n$-steps using the monotone membership boundary.

This is the classical analogue of [[Cyclic Group Extraction from a Semigroup Cycle]], but it is framed as an oracle reduction rather than a direct quantum period-finding algorithm.

## When to reach for it

Use it when a proposed cryptosystem tries to gain hardness by replacing a group with a one-generator periodic semigroup. If multiplication and equality are efficient, the real hardness is still only the cyclic-group DLP inside the eventual cycle.

## Complexity

Polynomial overhead in the representation size and in the logarithms of the cycle parameters, plus calls to a DLP oracle for the cyclic group $G$. With [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's group-DLP algorithm]], this gives a polynomial-time quantum algorithm for periodic semigroup DLP.

## Caveat

The trick is one-generator and periodic. It does not solve shifted semigroup DLP or multi-generator constructive membership, and it assumes canonical encodings plus efficient semigroup multiplication.

## Related notes

- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]
- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
- [[DLP-Oracle Order Recovery from Random Exponent Differences]]
- [[Idempotent Boundary Search in a Cyclic Semigroup]]
