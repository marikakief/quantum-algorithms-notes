> **Source:** Michael Ben-Or and Avinatan Hassidim, *The Bayesian Learner is Optimal for Noisy Binary Search (and Pretty Good for Quantum as Well)*, arXiv:quant-ph/0703231 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0703231)
> **Tags:** #search #ordered-list #binary-search #Bayesian #noisy-computation #query-complexity

---

## The computational problem

**Searching an ordered list:** Given $n$ elements $x_1 \geq \cdots \geq x_n$ and a special element $s$ with $x_1 \geq s \geq x_n$, find $i$ such that $x_i \geq s \geq x_{i+1}$. The oracle compares $s$ to any $x_i$.

Three variants:
1. **Classical noisy:** Each comparison returns the wrong answer with probability $1-p$.
2. **Batch noisy:** $k$ comparisons in parallel, each noisy.
3. **Quantum (exact):** Quantum oracle access to the comparison function.

---

## What the paper does

Two main contributions, one classical and one quantum:

**1. Optimal classical noisy binary search** via a Bayesian learner. Maintains a probability distribution $a_1, \ldots, a_n$ over possible positions of $s$. At each step, queries the "median" element (splitting the probability mass in half). Updates probabilities via Bayes' rule. Expected query count:

$$
\frac{\log n}{I(p)} + O\left(\frac{\log\log n \cdot \log(1/\delta)}{I(p)}\right)
$$

where $I(p) = 1 + p\log p + (1-p)\log(1-p)$ is the mutual information per query. This is optimal up to $\log\log n$ additive terms.

**2. Improved quantum search in an ordered list.** Uses the Bayesian noisy search framework to optimally exploit the Farhi-Goldstone-Gutmann-Sipser (FGGS) greedy quantum algorithm as a subroutine.

**Theorem 1.2:** The expected quantum query complexity of searching an ordered list is less than $0.32\log_2 n$.

This improves on the previous best of $0.433\log_2 n$ (Childs-Landahl-Parrilo 2006).

---

## The classical Bayesian algorithm

### Core idea

Maintain a probability vector $(a_1, \ldots, a_n)$ where $a_i = \Pr(x_i \geq s \geq x_{i+1})$. Initially uniform: $a_i = 1/n$.

At each step:
1. If any $a_i \geq \varepsilon_{\text{par}}$, we're concentrated enough — recurse on a small neighbourhood.
2. Otherwise, find index $i$ splitting the probability mass roughly in half: $\sum_{j \leq i} a_j \approx 1/2$.
3. Query $f(i)$ (noisy), update via Bayes' rule.

The Bayes update for $f(i) = 0$ (meaning $x_i < s$ with probability $p$):

$$
a_j \leftarrow \begin{cases} \frac{p \cdot a_j}{pq + (1-p)(1-q)} & j \leq i \\ \frac{(1-p) \cdot a_j}{pq + (1-p)(1-q)} & j > i \end{cases}
$$

where $q = \sum_{j \leq i} a_j$.

### Why it's optimal

Each query at the "half-probability" point extracts close to $I(p)$ bits of information (the maximum possible from a binary noisy channel). The total information needed to identify $s$ is $\log n$ bits, giving $\log n / I(p)$ queries.

The key lemma: the expected information gain per query is at least $I(p)(1 - 1/(3\log n))$ when $\varepsilon_{\text{par}} = \sqrt{1/(24\log n)}$. This comes from bounding the entropy $H(1/2 + \varepsilon_{\text{par}}(1-2p))$ using the inequality $H(1/2 + x) \geq 1 - 4x^2$.

---

## The quantum algorithm

### Strategy

Use the FGGS greedy quantum algorithm on a constant-size sublist as a subroutine inside the Bayesian framework.

The FGGS algorithm, given a sorted list of $K$ elements and $t$ quantum queries, outputs a quantum state where the probability of each position $j$ is a known distribution $p_0, p_1, \ldots, p_{K-1}$ (independent of the position of $s$, by translational invariance). Measuring gives a noisy answer.

This is exactly the input format for the generalised noisy search: we get a noisy oracle with known error distribution $(p_0, \ldots, p_{K-1})$, and the information per quantum subroutine call is $I(p_0, \ldots, p_{K-1}) = \log(K) - H(p_0, \ldots, p_{K-1})$.

### Best parameters found

Using $K = 223$ elements and $t = 6$ quantum queries gives $I(p_0, \ldots, p_{K-1}) = 18.5625$ bits of information per subroutine call of 6 queries, i.e., $18.5625/6 \approx 3.094$ bits per query. Since $\log_2 n$ bits are needed:

$$
\text{Expected queries} = \frac{6 \log_2 n}{18.5625} \approx 0.323 \log_2 n
$$

Compare: classical binary search needs $\log_2 n$ queries. The quantum advantage here is a constant-factor improvement, not a complexity class separation.

---

## Key results

| Result | Query complexity |
|---|---|
| Classical optimal (noiseless) | $\log_2 n$ |
| Classical optimal (noisy, error $1-p$) | $\log n / I(p) + O(\log\log n)$ |
| Best prior quantum (Childs-Landahl-Parrilo) | $0.433 \log_2 n$ |
| **This paper (quantum)** | $< 0.32 \log_2 n$ |
| Quantum lower bound (Høyer-Neerbek-Shi) | $\frac{\ln 2}{\pi}\log_2 n \approx 0.221 \log_2 n$ |

The gap between $0.32$ and $0.221$ remains open.

---

## Quantum lower bounds

**Theorem 4.2:** Any quantum algorithm finding the correct element with probability $\geq 1 - \delta$ needs at least $\frac{\ln 2}{\pi}((1-\delta)\log k) - O(\delta)$ queries.

This is proved by plugging the quantum subroutine into the Bayesian framework and using the information-theoretic bound: no quantum algorithm can extract more than $\pi/\ln 2$ bits per query (from Høyer-Neerbek-Shi).

---

## Comparison with prior work

| Paper | Result |
|---|---|
| Farhi-Goldstone-Gutmann-Sipser (1999) | $0.53\log_2 n$ via greedy algorithm on lists of 52 |
| Childs-Landahl-Parrilo (2006) | $0.433\log_2 n$ via SDP-optimised algorithm on lists of 605 |
| Høyer-Neerbek-Shi (2002) | Lower bound $\frac{\ln 2}{\pi}\log_2 n \approx 0.221\log_2 n$ |
| **This paper (2007)** | $< 0.32\log_2 n$ via Bayesian framework + FGGS on lists of 223 |

---

## Limits / caveats

- The quantum speedup is a constant factor improvement on $\log n$, not a complexity class separation. Ordered search is inherently $\Theta(\log n)$ for both classical and quantum — only the constant matters.
- The lower bound gap ($0.32$ vs $0.221$) remains open. The optimal constant for quantum ordered search is unknown.
- The algorithm is somewhat implicit: the FGGS subroutine is optimised numerically for specific list sizes ($K = 223$, $t = 6$), not derived from a clean closed-form.
- The Bayesian approach is elegant but the connection between noisy classical search and quantum search relies on treating the quantum measurement output as a noisy classical oracle — which is valid but obscures the quantum mechanics.

---

## Reusable ideas

1. **[[Bayesian Learner for Noisy Binary Search]]:** Maintain a probability distribution over positions, query at the median of the distribution, update via Bayes' rule. Achieves the information-theoretic optimal rate $I(p)$ bits per query. The key insight: myopic (greedy) Bayesian updating is optimal for noisy binary search — you don't need complex lookahead strategies.

2. **[[Quantum Subroutine as Noisy Classical Oracle]]:** A quantum algorithm that produces a known error distribution $(p_0, \ldots, p_{K-1})$ can be used as a subroutine in classical noisy search. The information gain per subroutine call is $I(p_0, \ldots, p_{K-1})$, and the total search cost is $\log n / I(p_0, \ldots, p_K)$ subroutine calls. This bridges quantum query algorithms and classical information theory.

---

## References within this paper

- Farhi, Goldstone, Gutmann & Sipser (1999) — the FGGS quantum ordered search algorithm
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer, Brassard, Høyer & Tapp (1998)]] — tight bounds on unstructured search
- Høyer, Neerbek & Shi (2002) — quantum lower bound for ordered search
- Childs, Landahl & Parrilo (2006) — improved quantum ordered search via SDP
- Ambainis (1999) — lower bound for quantum ordered search

---

## Cross-links

### Paper notes
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] — the unstructured search analogue
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — Grover search (different problem: unstructured vs ordered)
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — query complexity framework

### Trick cards
- [[Bayesian Learner for Noisy Binary Search]] — the classical algorithm
- [[Quantum Subroutine as Noisy Classical Oracle]] — the quantum-classical bridge
