# Grover Search over Discrete-Log Oracles

> **Source:** Wim van Dam and Igor E. Shparlinski, arXiv:0804.1109
> **Tags:** #trick #grover-search #discrete-logarithm #finite-fields

## What it does

Turns a two-variable exponential equation into a one-variable quantum search by making the predicate a discrete-log computation.

## The trick

For an equation over $\mathbb F_q^*$,

$$
a f^x+b g^y=c,
$$

fix a candidate $y$ and rearrange:

$$
f^x=a^{-1}(c-bg^y).
$$

The predicate for the outer search is:

1. compute $u_y=a^{-1}(c-bg^y)$;
2. decide whether $u_y\in\langle f\rangle$;
3. if yes, compute $x=\log_f(u_y)$ and verify $a f^x+b g^y=c$.

On a quantum computer, steps 2--3 are done in $(\log q)^{O(1)}$ time using [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's discrete-log algorithm]]. The outer loop over $y$ is then searched with [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]].

If there are $R$ candidate $y$ values and at least one is marked, the cost is

$$
O(\sqrt R\, (\log q)^{O(1)}).
$$

In [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]], character-sum windowing sets $R\approx q^{3/2}/s$, and the balancing argument gives the worst-case $q^{3/8}(\log q)^{O(1)}$ runtime.

## When to reach for it

- One variable appears as a power of a known base, so fixing the other variable leaves a discrete-log instance.
- There is a proof that a restricted candidate set contains a solution.
- You want a modular way to combine algebraic quantum subroutines with amplitude search.

## Complexity

For $R$ candidates and a polylogarithmic discrete-log oracle, the quantum query count is $O(\sqrt R)$. If the discrete-log step is classical and generic, the predicate cost is $s^{1/2}(\log q)^{O(1)}$ instead, which can wipe out the search gain.

## Caveat

This is not a route to polylogarithmic time by itself. If the only available candidate set has size $q^\alpha$, Grover reduces the exponent by a factor of two but still leaves exponential-in-$\log q$ runtime.

## Related notes

- [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]]
- [[Character-Sum Windowing for Exponential Congruence Search]]
- [[Solution-Density Grover Search from Character-Sum Counts]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
