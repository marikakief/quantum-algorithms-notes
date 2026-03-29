# Quantum Subroutine as Noisy Classical Oracle

> **Source:** Ben-Or & Hassidim, arXiv:quant-ph/0703231
> **Tags:** #trick #quantum-classical #search #information-theory

## What it does

Bridges quantum query algorithms and classical information theory by treating the measurement output of a quantum subroutine as a noisy classical oracle with known error distribution.

## The trick

A quantum algorithm that searches a sorted list of $K$ items using $t$ queries produces a measurement outcome from a distribution $(p_0, p_1, \ldots, p_{K-1})$ where $p_j$ is the probability of reporting position $j$ when the correct answer is position $0$ (by translational invariance of the FGGS construction).

The information gained per subroutine call is:

$$
I(p_0, \ldots, p_{K-1}) = \log K - H(p_0, \ldots, p_{K-1})
$$

where $H$ is the Shannon entropy. The rate is $I / t$ bits per quantum query.

Plug this into the [[Bayesian Learner for Noisy Binary Search|Bayesian noisy search framework]] to search a list of $n$ elements:

$$
\text{Expected queries} = \frac{t \cdot \log n}{I(p_0, \ldots, p_{K-1})}
$$

Optimise over $K$ and $t$ to minimise the ratio $t / I$.

Best known: $K = 223$, $t = 6$ gives $I = 18.5625$, yielding $< 0.32\log_2 n$ queries total.

## When to reach for it

- Combining a quantum subroutine (that searches a constant-size list) with classical divide-and-conquer.
- Optimising constant factors in quantum query complexity when the asymptotic scaling is $\Theta(\log n)$.
- Any situation where a quantum procedure outputs a noisy classical answer and you want to use it adaptively.

## Complexity

The total cost is $\frac{t \cdot \log n}{I(p_0, \ldots, p_{K-1})} + O(\text{lower order})$. The quantum subroutine is called $O(\log n)$ times.

## Caveat

Only useful when the quantum and classical complexities have the same asymptotic form ($\Theta(\log n)$ for ordered search). For problems where quantum gives a different asymptotic speedup (e.g., $\sqrt{N}$ vs $N$ for unstructured search), this framework doesn't apply.

The optimal choice of $K$ and $t$ requires numerical optimisation of the FGGS algorithm — there's no clean closed-form solution.

## Related notes
- [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]]
- [[Bayesian Learner for Noisy Binary Search]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
