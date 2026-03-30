# Adversary Method for Quantum Query Lower Bounds

> **Source:** Ambainis, arXiv:quant-ph/0002066
> **Tags:** #trick #query-complexity #lower-bound #adversary-method

## What it does

Proves lower bounds on quantum query complexity by running the algorithm on a superposition of inputs and tracking how fast entanglement grows between the algorithm and the input register.

## The trick

Let $f : \{0,1\}^N \to \{0,1\}$. Choose sets $X \subseteq f^{-1}(0)$, $Y \subseteq f^{-1}(1)$, and a relation $R \subseteq X \times Y$ satisfying:
- Every $x \in X$ has $\geq m$ partners in $Y$
- Every $y \in Y$ has $\geq m'$ partners in $X$
- For each $x$ and index $i$: at most $l$ partners $y$ with $x_i \neq y_i$
- For each $y$ and index $i$: at most $l'$ partners $x$ with $x_i \neq y_i$

Then $Q_2(f) = \Omega\!\left(\sqrt{mm'/ll'}\right)$.

**Why it works:** Run the algorithm on $|0\rangle \otimes \frac{1}{\sqrt{|S|}} \sum_{x \in S} |x\rangle$. Correctness forces the density matrix $\rho$ on the input register to lose off-diagonal weight between inputs with different function values. Each oracle query can only change the relevant off-diagonal entries by $O(\sqrt{ll'})$ (because a query on index $i$ only affects pairs where $x_i \neq y_i$, and conditions 3–4 limit how many such pairs exist). The initial off-diagonal weight is $\Omega(\sqrt{mm'})$, so $\Omega(\sqrt{mm'/ll'})$ queries are needed.

## When to reach for it

- You want a lower bound on bounded-error quantum query complexity of a total or partial Boolean function.
- The function has a natural "yes/no" structure where you can identify many related input pairs that differ in few positions.
- You need something stronger than the $\Omega(\sqrt{\text{bs}(f)})$ block sensitivity bound.
- The [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]] doesn't give the bound you want (or you want a combinatorial proof).

## Complexity

No algorithmic cost — this is a proof technique, not an algorithm.

## Caveat

The adversary method (including the [[Weight Scheme Adversary Method|weighted version]]) is bounded above by $O(\sqrt{C_0(f) \cdot C_1(f)})$ for total functions, where $C_0, C_1$ are certificate complexities. For element distinctness ($C_1 = 2$, $C_0 = N$), this gives only $\Omega(\sqrt{N})$ — the true lower bound is $\Omega(N^{2/3})$, proved by the [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]].

The *negative-weight* / *general adversary bound* (Høyer-Lee-Špalek 2007) removes this barrier and is tight for all Boolean functions.

## Related notes
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]]
- [[Weight Scheme Adversary Method]]
- [[Hybrid Argument Lower Bound for Quantum Search]]
- [[Query Magnitude Hybrid Argument for Oracle Lower Bounds]]
