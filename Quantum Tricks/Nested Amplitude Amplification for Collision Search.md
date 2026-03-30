# Nested Amplitude Amplification for Collision Search

> **Source:** Buhrman, Dürr, Heiligman, Høyer, Magniez, Santha, de Wolf, arXiv:quant-ph/0007016 (2001)
> **Tags:** #trick #collision #amplitude-amplification #element-distinctness #claw-finding

## What it does

Finds a collision (or claw) in an arbitrary function $f: [N] \to \mathbb{Z}$ using $O(N^{3/4} \log N)$ comparisons, without any structural promise on $f$.

## The trick

Two layers of quantum search, with classical sorting sandwiched in between:

1. **Sample:** Pick a random subset $A \subseteq [N]$ of size $\ell$ and a random subset $B \subseteq [N]$ of size $\ell^2$
2. **Sort:** Classically sort $A$ by function values — $O(\ell \log \ell)$ comparisons
3. **Inner Grover:** For each $b \in B$, use binary search on sorted $A$ to check if $f(b) \in \{f(a) : a \in A\}$. [[Standard Amplitude Amplification|Grover search]] over $B$ finds a collision in $A \times B$ in $O(\ell \log \ell)$ comparisons
4. **Outer amplification:** Steps 1–3 succeed with probability $\Theta(\ell^3/N^2)$. [[Standard Amplitude Amplification|Amplitude amplification]] boosts this with $O(\sqrt{N^2/\ell^3})$ repetitions

Total: $O(\sqrt{N^2/\ell} \cdot \log \ell)$. Setting $\ell = \sqrt{N}$ gives $O(N^{3/4} \log N)$.

The key insight: sorting creates an efficient lookup structure that makes each inner Grover check cost only $O(\log \ell)$ instead of $O(\ell)$. The outer amplification handles the small probability that the collision lands in the random subsets.

## When to reach for it

- Collision or claw-finding where the function has no exploitable structure (not 2-to-1, not ordered)
- When you want lower space than [[Walk on the Johnson Graph for Subset Search|quantum walk approaches]]: this uses $O(\sqrt{N})$ space vs $O(N^{2/3})$ for the walk
- When simplicity matters more than optimality — the technique is conceptually straightforward compared to quantum walks
- Claw-finding between two different functions $f: [N] \to \mathbb{Z}$ and $g: [M] \to \mathbb{Z}$

## Complexity

$O(N^{3/4} \log N)$ comparisons for element distinctness. $O(N^{1/2} M^{1/4} \log N)$ for claw-finding with $N \leq M \leq N^2$. Space: $O(\sqrt{N})$.

## Caveat

Superseded by [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis's $O(N^{2/3})$ walk algorithm]] for element distinctness. The walk avoids the sorting overhead entirely by amortising the cost of moving between adjacent subsets. However, this technique remains useful when space is constrained to $O(\sqrt{N})$, where the walk can't operate at its optimal regime.

The log factor from sorting is inherent to this approach and can't be removed without changing the algorithmic structure.

## Related notes

- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Walk on the Johnson Graph for Subset Search]]
- [[Birthday-Grover Space-Time Tradeoff]]
- [[Classical Preprocessing plus Grover Search]]
