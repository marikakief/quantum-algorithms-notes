# Classical Preprocessing plus Grover Search

> **Source:** Brassard, Høyer & Tapp, arXiv:quant-ph/9705002 (1997)
> **Tags:** #trick #collision #Grover #hybrid #query-complexity

## What it does

Combines a classical preprocessing phase (build a table of $k$ oracle evaluations) with a Grover search phase (find an element interacting with the table). Balancing the two phases gives query complexity $O(N^{1/3})$ for collision-type problems — beating both pure classical ($\sqrt{N}$) and pure Grover ($\sqrt{N}$) approaches.

## The trick

1. **Classical phase:** Evaluate $F$ on a set $K$ of $k$ inputs. Store the results in a sorted table $L$. Cost: $k$ queries, $\Theta(k)$ space.
2. **Quantum phase:** Define $H(x) = 1$ iff $F(x)$ matches something in $L$ (and $x \notin K$). Run [[Standard Amplitude Amplification|Grover search]] for $H$. There are $t = \Theta(rk)$ solutions (for $r$-to-1 functions), so Grover costs $O(\sqrt{N/(rk)})$ queries.
3. **Total:** $k + O(\sqrt{N/(rk)})$. Minimise over $k$: set $k = (N/r)^{1/3}$, getting $O((N/r)^{1/3})$.

Each Grover oracle call evaluates $F$ once and does a binary search in $L$ — the table lives in quantum-accessible memory (QRAM or quantum-readable classical memory).

## When to reach for it

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Collision finding]] in $r$-to-1 functions
- Claw finding between two functions
- Any problem where you can turn "find an element related to a known set" into a Grover search
- Hybrid quantum-classical algorithms where classical precomputation reduces the Grover search space

## Complexity

Total queries: $O(N^{1/3})$ at the sweet spot. Space: $\Theta(N^{1/3})$ for the table.

General tradeoff: $ST^2 \geq N$ where $S$ = space and $T$ = queries.

## Caveat

The table must be **quantum-accessible** — Grover's oracle needs to check against $L$ in superposition. This requires either QRAM or embedding the sorted table into the quantum circuit. The QRAM model is powerful but physically controversial.

The $N^{1/3}$ bound is optimal for $r$-to-1 collision (Aaronson-Shi 2004), but for unstructured element distinctness (no $r$-to-1 promise), the tight bound is $\Theta(N^{2/3})$ via quantum walks (Ambainis 2007).

## Related notes

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]
- [[Birthday-Grover Space-Time Tradeoff]]
- [[Standard Amplitude Amplification]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
