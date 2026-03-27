
> **Source:** Farhi, Goldstone & Gutmann, arXiv:1411.4028 (2014)
> **Tags:** #trick #QAOA #classical-simulation #locality #variational

## What it does

For variational quantum circuits of fixed depth $p$ on bounded-degree problems, computes the optimal variational parameters entirely classically — in time independent of the problem size $n$.

## The trick

The expectation value $F_p(\boldsymbol{\gamma}, \boldsymbol{\beta}) = \langle \boldsymbol{\gamma}, \boldsymbol{\beta}| C |\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle$ decomposes as a sum over local terms (e.g., edges for MaxCut). Each term's contribution depends only on the subgraph within distance $p$ of that term.

For bounded-degree graphs, these subgraphs have bounded size (at most $2[(v-1)^{p+1} - 1]/[(v-1) - 1]$ vertices for maximum degree $v$). Group terms by subgraph isomorphism type:

$$F_p(\boldsymbol{\gamma}, \boldsymbol{\beta}) = \sum_g w_g \, f_g(\boldsymbol{\gamma}, \boldsymbol{\beta})$$

where $w_g$ counts how many terms have type $g$ (read off the graph classically), and $f_g$ is computed by simulating a small quantum system of $n$-independent size.

The angles maximising $F_p$ are found by optimising this classical function. The quantum computer is needed only for *sampling* from the optimal state.

## When to reach for it

- Any [[Alternating Operator Ansatz|QAOA]]-type algorithm at fixed depth on bounded-degree problems
- Analysing the performance of variational circuits without running quantum hardware
- Proving worst-case approximation bounds (as in the $0.6924$ result for MaxCut on 3-regular graphs)
- Identifying when variational circuits are classically simulable vs. when they genuinely need quantum resources

## Complexity

Classical preprocessing: polynomial in the number of subgraph types (finite for fixed $p$ and bounded degree), times $2^{O(v^p)}$ for simulating each subgraph. The only $n$-dependence is reading off the weights $w_g$, which is linear.

## Caveat

Only works for fixed $p$. When $p$ grows with $n$, the subgraphs span the entire graph and classical simulation becomes exponential. Also limited to bounded-degree problems — dense graphs have subgraphs that grow with $n$ even at $p = 1$.

## Related notes

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — where this technique was introduced
- [[Alternating Operator Ansatz]] — the circuit structure this applies to
