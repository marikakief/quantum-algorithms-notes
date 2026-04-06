# Embedding Hard Problems into Free Base Coordinates

> **Source:** Ben-David, Childs, Gilyén, Kretschmer, Podder, Wang, arXiv:2006.12760
> **Tags:** #trick #query-complexity #symmetry #quantum-speedup #oracle-separation

## What it does

For any permutation group $G$ with small minimal base size $b(G) = n^{o(1)}$, constructs a partial function that is $G$-symmetric and has a super-polynomial quantum speedup, by embedding an arbitrary hard promise problem into the "free" base coordinates.

## The trick

Given a permutation group $G$ on $[n]$ with minimal base $B$ of size $b(G)$:

1. Start with any partial function $f: P \to \{0,1\}$ ($P \subseteq \Sigma^n$) that has $Q(f) = n^{o(1)}$ and $R(f) = n^{\Omega(1)}$ (e.g., Simon's problem, Forrelation).

2. Define a new function $g$ on strings $x \in (\Sigma \times [n])^n$: the string $x$ is in the promise if there exists $\pi \in G$ such that $x_{\pi(i)} = (y_i, i)$ for all $i$, where $y \in P$. Set $g(x) = f(y)$.

3. **$g$ is $G$-symmetric** by construction (applying $\sigma \in G$ just permutes the encoding).

4. **Quantum upper bound:** Query the $b(G)$ base coordinates to determine $\pi$ (since $\pi$ is uniquely determined by its values on the base), then solve $f$ on the decoded input. Total: $Q(g) \le Q(f) + b(G)$.

5. **Classical lower bound:** $R(g) \ge R(f)$, because the identity permutation case is a subproblem identical to $f$.

## When to reach for it

- Proving that a class of permutation groups *does* allow super-polynomial quantum speedups
- Constructing oracle separations for promise problems with prescribed symmetry
- Understanding the role of base size in quantum-classical gaps

## Complexity

$Q(g) = Q(f) + b(G)$. If $b(G) = n^{o(1)}$ and $f$ has exponential quantum speedup (e.g., Simon: $Q = O(\log n)$, $R = 2^{\Omega(n)}$), then $g$ also has super-polynomial speedup.

## Caveat

Requires large alphabets — the alphabet size is $|\Sigma| \cdot n$, which can be larger than $n$ itself. Whether the construction works over a Boolean alphabet for all groups is open (some permutation groups can't be realised as symmetry groups of Boolean functions). Also, only works for promise problems (partial functions), not total functions.

## Related notes
- [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]]
- [[Well-Shuffling Reduction for Quantum Speedup Bounds]]
- [[Adversary Method for Quantum Query Lower Bounds]]
