# Well-Shuffling Reduction for Quantum Speedup Bounds

> **Source:** Ben-David, Childs, Gilyén, Kretschmer, Podder, Wang, arXiv:2006.12760
> **Tags:** #trick #query-complexity #symmetry #quantum-speedup #lower-bound

## What it does

Converts the question "can functions symmetric under group $G$ have super-polynomial quantum speedups?" into the concrete distinguishing problem "can a quantum algorithm distinguish random permutations from $G$ from small-range functions?"

## The trick

1. **Define $\mathrm{cost}(G, r)$:** the quantum query complexity of distinguishing $G \subseteq [n]^n$ (viewed as permutation strings) from $D_{n,r}$ (strings using at most $r$ distinct symbols).

2. **Call $G$ well-shuffling** if $\mathrm{cost}(G, r) = r^{\Omega(1)}$.

3. **Main theorem:** If $G$ is well-shuffling with power $a$, then $R(f) = O(Q(f)^a)$ for every $f$ symmetric under $G$.

4. **Proof mechanism:** A $T$-query quantum algorithm $Q$ for $f$ can't distinguish $\pi \in G$ from $\alpha \in D_{n,r}$ when $r \ll \mathrm{cost}(G, r)$. So $Q$ works on $x \circ \alpha$ too, and a classical algorithm simulates this by querying only the $r$ relevant bits.

5. **Toolkit for proving well-shuffling:**
   - $S_n$ is well-shuffling (power 3) via collision lower bound
   - Power action $G^{(k)}$: $\mathrm{cost}(G^{(k)}, r^k) \ge \mathrm{cost}(G, r)/k$
   - Block systems: $\mathrm{cost}(H, r) \ge \mathrm{cost}(G, r)$ when $H$ acts on blocks of $G$
   - Product: $\mathrm{cost}(G_1 \times G_2, r^2) \ge \min\{\mathrm{cost}(G_1, r), \mathrm{cost}(G_2, r)\}$
   - Merger: $\mathrm{cost}(\langle G, H\rangle, r) \ge \mathrm{cost}(G, r)$

## When to reach for it

- Proving that a class of symmetric problems can't have exponential quantum speedups
- Establishing polynomial relationships between quantum and classical query complexities for structured problems
- Connecting symmetry groups to quantum lower bounds without working directly with adversary methods

## Complexity

The reduction itself is free. The cost of the resulting classical simulation is $O(T^a)$ queries where $T = Q(f)$ and $a$ is the well-shuffling power.

## Caveat

Only applies in the query complexity model. Doesn't say anything about time complexity beyond query complexity. Also, the power $a$ can be large for highly symmetric but non-trivially structured groups ($a = 3p$ for $p$-uniform hypergraphs), so the polynomial relationship can be loose.

## Related notes
- [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]]
- [[Embedding Hard Problems into Free Base Coordinates]]
- [[Adversary Method for Quantum Query Lower Bounds]]
