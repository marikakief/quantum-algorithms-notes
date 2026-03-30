# Quantum Binary Search via Approximate Counting

> **Source:** Nayak and Wu, arXiv:quant-ph/9804066 (1999)
> **Tags:** #trick #quantum-algorithms #search #counting #order-statistics #median

## What it does
Combines quantum uniform sampling, quantum approximate counting, and random-pivot binary search to find approximate order statistics (median, $k$th-smallest) with near-optimal query complexity.

## The trick

**Goal:** Find a $\Delta$-approximate $k$th-smallest element of $n$ numbers.

**Structure:** Random-pivot binary search over the input values, using two quantum subroutines:

1. **Sampler $S(i,j)$:** Given indices $i, j$ bounding a range, uniformly sample an element with value between $x_i$ and $x_j$. Implemented via [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp]] generalised search. Cost: $O(\sqrt{n/\Delta} \cdot \log\log N)$ queries (there are always $\geq \Omega(\Delta)$ valid elements).

2. **Distinguisher $K'(i)$:** Determine whether $x_i$ has rank close to $k$, above $k + \Delta$, or below $k - \Delta$. Implemented via [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Mosca-Tapp]] approximate counting applied to the Boolean function "how many elements are less than $x_i$?" Cost: $O(N \cdot \log\log N)$ queries where $N = \sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta}$.

3. **Binary search wrapper:** Maintain indices $i, j$ bracketing the target rank. At each stage, sample a pivot and classify it. Expected $O(\log N)$ stages (proven by bounding $\sum_l p(l, -1, n) = O(\log N)$ via a recurrence on selection probabilities).

**Total:** $O(N \log N \log\log N)$ queries.

**Relaxation trick:** The distinguisher $K'$ is allowed to be non-deterministic in a "buffer zone" of width $\Delta/2$ around the target rank. This doesn't increase the expected number of stages (Lemma 3.4 — it can only cause the algorithm to converge to a $\Delta/2$-approximate answer before terminating, which is still correct).

## When to reach for it

- Computing approximate order statistics (median, quantiles, percentiles) of data accessible only through queries
- Any problem where you need to locate an element by rank in a sequence and classical binary search works but you want a quadratic-type speedup on the per-step query cost
- The algorithm works in the comparison tree model too (each comparison costs 4 oracle queries), making it applicable to comparison-based quantum computation

## Complexity
$O((\sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta}) \cdot \log(\cdot) \cdot \log\log(\cdot))$ queries. For the median ($k = n/2$, $\Delta = \varepsilon n/2$): $O(\frac{1}{\varepsilon} \log \frac{1}{\varepsilon} \log\log \frac{1}{\varepsilon})$.

## Caveat
The polylog factors come from error amplification ($\log\log N$ repetitions per stage) and the binary search structure ($\log N$ stages). Removing them to match the $\Omega(1/\varepsilon)$ lower bound exactly remains open. The sampler requires at least $\Omega(\Delta)$ valid elements in each search interval, which is guaranteed by the binary search invariant.

## Related notes
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] — the minimum-finding algorithm that inspired this approach
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] — uniform sampling subroutine
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]] — approximate counting subroutine
- [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]] — related quantum binary search techniques
