
> **Source:** Dürr & Høyer, arXiv:quant-ph/9607014 (1996)
> **Tags:** #trick #search #Grover #optimization #minimum-finding

## What it does
Finds the minimum of an unsorted table in $O(\sqrt{N})$ queries by iteratively using Grover search with an evolving threshold.

## The trick

Naively applying Grover to find the minimum seems like it should cost more than $O(\sqrt{N})$: you need multiple rounds (find something smaller than the current best, update, repeat), and the number of marked items changes each round.

The trick: start with a random threshold $y$, then repeatedly Grover-search for any item $j$ with $T[j] < T[y]$. When found, update $y \leftarrow j$. Use **exponential search** (Boyer-Brassard-Høyer-Tapp) because you don't know how many items are below threshold.

**Why it's still $O(\sqrt{N})$ total:** When $t$ items remain below threshold, the expected search cost is $O(\sqrt{N/t})$. Summing over all threshold updates gives a harmonic-like series:

$$\sum_{t} \frac{\sqrt{N}}{\sqrt{t}} \approx \sqrt{N} \cdot \sum \frac{1}{\sqrt{t}} = O(\sqrt{N})$$

because each rank is visited with probability $1/r$ (uniform output of exponential search), and the weighted sum telescopes.

Set a time-out at $O(\sqrt{N})$ and the minimum is found with probability $\geq 1/2$.

## When to reach for it
- Finding the minimum (or maximum) of an unstructured set with quantum oracle access
- Any optimisation problem reducible to "find the best item in a list"
- As a subroutine in quantum gradient descent, combinatorial optimisation, or numerical algorithms

## Complexity
$O(\sqrt{N})$ queries, $O(c\sqrt{N})$ for success probability $\geq 1 - 2^{-c}$.

## Caveat
Requires coherent oracle access. The evolving threshold means the marking oracle changes between rounds — this is fine because the threshold is classical (just a stored index). Not useful when you only have sample access to $T$.

## Related notes
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Amplitude Amplification and Estimation]]
