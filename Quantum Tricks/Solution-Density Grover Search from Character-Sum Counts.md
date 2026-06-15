# Solution-Density Grover Search from Character-Sum Counts

> **Source:** Wim van Dam and Igor E. Shparlinski, arXiv:0804.1109
> **Tags:** #trick #grover-search #character-sums #finite-fields

## What it does

Uses a number-theoretic count of marked items to replace Grover's $O(\sqrt r)$ cost by $O(\sqrt{r/m})$.

## The trick

Grover search with multiple marked items finds a solution among $r$ candidates using

$$
O\left(\sqrt{\frac r m}\right)
$$

predicate calls when $m$ candidates are marked, using the Boyer--Brassard--Høyer--Tapp search variant.

The reusable move is to get $m$ from a character-sum estimate. For the exponential congruence

$$
a f^x+b g^y=c,
$$

with $0\leq x<s$ and $0\leq y<r$, van Dam--Shparlinski prove

$$
N_{a,b,c}(r,s)=\frac{rs}{q-1}+O(q^{1/2}\log q).
$$

When $st$ is large enough, choose

$$
r=\left\lfloor Cq^{3/2}s^{-1}(\log q)^{1/2}\right\rfloor\leq t
$$

so that the main term dominates the error. Then the number of marked $y$ values is

$$
m\geq \frac{rs}{2q},
$$

because each $y$ has at most one $x\in\{0,\ldots,s-1\}$ satisfying $f^x=a^{-1}(c-bg^y)$. The Grover cost becomes

$$
\sqrt{\frac r m}=O(q^{1/2}s^{-1/2}),
$$

which is at most

$$
q^{1/2}(st)^{-1/4}
$$

when $s\geq t$.

## When to reach for it

- A counting theorem gives a lower bound on the number of successful candidates.
- The predicate has a unique witness per outer candidate, or at least lets you convert solution count into marked-candidate count.
- The raw candidate set is large, but solution density is analyzable.

## Complexity

In the source paper, this gives the large-order quantum bound

$$
q^{1/2}(st)^{-1/4}(\log q)^{O(1)}
$$

under

$$
st>Cq^{3/2}(\log q)^{1/2}.
$$

A typical-case variant over $c$ works under the weaker condition

$$
st>q^{4/3}(\log q)^{2/3}.
$$

## Caveat

The count must be strong enough to dominate its error term. If the estimate only says the expected number is comparable to the uncertainty, the multiple-solution Grover speedup is not justified.

## Related notes

- [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]]
- [[Character-Sum Windowing for Exponential Congruence Search]]
- [[Grover Search over Discrete-Log Oracles]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
