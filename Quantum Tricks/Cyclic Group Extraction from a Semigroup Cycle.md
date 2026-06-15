# Cyclic Group Extraction from a Semigroup Cycle

> **Source:** Childs and Ivanyos, arXiv:1310.6238
> **Tags:** #trick #semigroups #discrete-log #group-extraction

## What it does

Turns the cyclic part of a one-generator semigroup into an explicit cyclic group, so group algorithms can be used without global inverses.

## The trick

Let $g$ be an element of a finite semigroup with index $t$ and period $r$:
$$
g^{t+r}=g^t.
$$
The cycle
$$
C=\{g^{t+j}:j\in\mathbb Z_r\}
$$
is a group under semigroup multiplication. If $s=-t\bmod r$, then
$$
e_C=g^{t+s}
$$
is the identity in $C$, because for any $j\geq t$ the exponent $t+s+j$ is congruent to $j$ modulo $r$ inside the cycle.

A generator of $C$ is
$$
h=g^{t+s+1},
$$
and its inverse is known explicitly:
$$
h^{-1}=g^{t+s+r-1}.
$$
Thus, even though the ambient semigroup has no inverses, the cycle has enough structure to run cyclic-group subroutines. For example, to compute the discrete log of $x\in C$, use Shor's hiding function
$$
f(a,b)=x^a h^{-b}=x^a g^{(t+s+r-1)b},
\qquad (a,b)\in\mathbb Z_r^2.
$$
Then convert back to the original exponent by
$$
\log_g x=t+(s+\log_h x\bmod r).
$$

## When to reach for it

- A one-generator semigroup has already had its index and period identified.
- The target lies in the cycle rather than the tail.
- A group algorithm only needs multiplication, equality, and an inverse for the chosen generator.

## Complexity

After $t$ and $r$ are known, all needed powers are computed by repeated squaring in $\operatorname{poly}(\log(t+r))$ semigroup multiplications. The group subroutine then has its usual cyclic-group cost; for [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor discrete log]], this is polynomial in $\log r$.

## Caveat

This extracts a group only from the eventual cycle of a single generator. It does not make a multi-generator semigroup group-like, and it does not solve shifted equations $x=yg^a$ when $y$ prevents direct access to the cycle group element.

## Related notes

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]
- [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]]
- [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]]
- [[Shifted Semigroup Log as Dihedral Hidden Shift]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
