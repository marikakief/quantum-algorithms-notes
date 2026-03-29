# Bayesian Learner for Noisy Binary Search

> **Source:** Ben-Or & Hassidim, arXiv:quant-ph/0703231
> **Tags:** #trick #search #Bayesian #information-theory #noisy-computation

## What it does

Finds an element in a sorted list of $n$ items using $\log n / I(p) + O(\log\log n)$ noisy comparisons — information-theoretically optimal up to lower-order terms.

## The trick

Maintain a probability vector $(a_1, \ldots, a_n)$ where $a_i = \Pr(\text{target is at position } i)$. Initially $a_i = 1/n$.

At each step:
1. Find the index $i$ that splits the probability mass roughly in half: $\sum_{j \leq i} a_j \approx 1/2$.
2. Query the noisy oracle at $i$.
3. Update all probabilities via Bayes' rule:
   - If oracle says $x_i \geq s$: multiply $a_j$ by $p$ for $j \leq i$, by $1-p$ for $j > i$, normalise.
   - If oracle says $x_i < s$: multiply $a_j$ by $1-p$ for $j \leq i$, by $p$ for $j > i$, normalise.
4. When some $a_i \geq \varepsilon_{\text{par}}$, recurse on a small neighbourhood.

**Why it's optimal:** Each query at the half-probability point extracts close to $I(p) = 1 + p\log p + (1-p)\log(1-p)$ bits of information — the mutual information of a binary symmetric channel with crossover probability $1-p$. After $\log n / I(p)$ queries, the entropy drops below $\log(1/\varepsilon_{\text{par}})$ and the target is localised.

The key technical lemma: with $\varepsilon_{\text{par}} = \sqrt{1/(24\log n)}$, the information gain per query is at least $I(p)(1 - 1/(3\log n))$.

## When to reach for it

- Searching sorted data with unreliable comparisons (noisy sensors, biological assays, etc.).
- As the outer loop for quantum ordered search: treat quantum measurement outcomes as noisy oracle answers.
- Any adaptive estimation problem where you're trying to localise a parameter using noisy binary questions.

## Complexity

$\log n / I(p) + O(\log\log n \cdot \log(1/\delta) / I(p))$ queries for success probability $1 - \delta$. Runtime $O(\log^2 n)$ classically (or $O(\log n \cdot \log\log n)$ with balanced trees).

## Caveat

Requires the noise model to be known ($p$ must be given). Adversarial noise models need different algorithms. The $O(\log\log n)$ additive term is an artefact of the recursive step and probably not tight.

## Related notes
- [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]]
- [[Quantum Subroutine as Noisy Classical Oracle]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
