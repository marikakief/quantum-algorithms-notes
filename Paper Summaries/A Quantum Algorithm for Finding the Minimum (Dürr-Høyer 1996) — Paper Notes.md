> **Source:** Christoph Dürr and Peter Høyer, *A Quantum Algorithm for Finding the Minimum*, arXiv:quant-ph/9607014 (1996)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9607014)
> **Tags:** #quantum-algorithm #search #Grover #minimum-finding #oracle

---

## The problem

Given an unsorted table $T[0..N-1]$ of $N$ items from an ordered set, find the index $y$ such that $T[y]$ is the minimum. Classically this requires $\Theta(N)$ probes. The BBBV lower bound implies any quantum algorithm needs $\Omega(\sqrt{N})$ queries.

## What the paper does

A clean $O(\sqrt{N})$ quantum algorithm for minimum-finding, matching the lower bound up to constants. Succeeds with probability $\geq 1/2$ (boosted to $\geq 1 - 1/2^c$ by $c$ repetitions). Uses Grover's exponential search as a subroutine.

Two pages. No fat.

---

## The algorithm

**Quantum Minimum Searching:**

1. Choose a threshold index $y$ uniformly at random from $\{0, \ldots, N-1\}$.
2. Repeat (with a time-out of $22.5\sqrt{N} + 1.4 \lg^2 N$ steps):
   - (a) Prepare $\frac{1}{\sqrt{N}} \sum_j |j\rangle |y\rangle$. Mark every index $j$ with $T[j] < T[y]$.
   - (b) Run the quantum exponential searching algorithm (Boyer-Brassard-Høyer-Tapp).
   - (c) Measure: get outcome $y'$. If $T[y'] < T[y]$, set $y \leftarrow y'$.
3. Return $y$.

The idea is simple: iteratively use Grover search to find *any* item smaller than the current threshold, then update the threshold. The exponential search variant handles the unknown number of marked items (you don't know how many items are below the threshold).

---

## Why it's $O(\sqrt{N})$

The key insight is a probabilistic analysis across all iterations.

**Lemma 1:** When the algorithm is searching among $t$ marked items (items smaller than $T[y]$), the probability that the item of rank $r$ is ever chosen as the threshold is $p(t, r) = 1/r$, independent of $N$. Proved by induction — the uniform output of exponential search is what makes it work.

**Lemma 2:** The expected total time before $y$ holds the minimum is:

$$\sum_{r=2}^{N} \frac{1}{r+1} \cdot \frac{9}{2} \sqrt{\frac{N}{r-1}} \leq \frac{45}{4}\sqrt{N}$$

Each iteration with $t$ marked items takes expected $\frac{9}{2}\sqrt{N/t}$ steps (exponential search cost). Sum over all possible intermediate thresholds, weighted by their probability, and the harmonic-like series telescopes to $O(\sqrt{N})$.

The time-out at $2m_0 \approx 22.5\sqrt{N}$ guarantees success probability $\geq 1/2$ by Markov's inequality.

---

## Key results

| Result | Statement |
|---|---|
| Query complexity | $O(\sqrt{N})$ to find the minimum with probability $\geq 1/2$ |
| Boosted | $O(c\sqrt{N})$ for success probability $\geq 1 - 1/2^c$ |
| Optimality | Matches $\Omega(\sqrt{N})$ lower bound from BBBV |
| Non-distinct values | Same algorithm, same bound (inequality replaces equality in Lemma 1) |

---

## Why it matters

This is the canonical application of Grover search to optimisation. Every quantum speedup for an optimisation problem that works by "Grover over the feasible set with an evolving threshold" is essentially this paper's idea. It shows up in:

- Quantum algorithms for combinatorial optimisation (minimum spanning tree, shortest path, etc.)
- Quantum speedups for numerical optimisation (as the search subroutine)
- The Gilyén-Arunachalam-Wiebe gradient optimisation paper uses Dürr-Høyer as a building block

The analysis is also a nice example of how to handle Grover search when the number of marked items is unknown and changes during the algorithm.

---

## Limits / caveats

- The $O(\sqrt{N})$ speedup is quadratic, not exponential. For structured problems, better algorithms often exist.
- Requires coherent oracle access to $T$ — can't just use classical samples.
- The time-out-based approach means the algorithm doesn't "know" when it's found the minimum. It just runs for a fixed time and hopes. The probability analysis says this works.
- For approximate minimum (finding an item within $\varepsilon$ of the minimum), the same approach works but the analysis is slightly different.

---

## Reusable ideas

1. [[Grover Search with Evolving Threshold for Minimum Finding]] — iteratively Grover-search for items below the current best, updating the threshold. The unknown number of marked items is handled by exponential search. Total cost $O(\sqrt{N})$ despite the iterations.

---

## Cross-links

### Paper notes
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the core search subroutine
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]] — gradient + Dürr-Høyer gives quantum gradient descent
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]] — uses Dürr-Høyer as optimisation subroutine

### Trick cards
- [[Grover Search with Evolving Threshold for Minimum Finding]]
- [[Amplitude Amplification and Estimation]]
