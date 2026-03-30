# Symmetrisation Reduction for Symmetric Boolean Functions

> **Source:** Minsky & Papert, *Perceptrons* (1968); applied in Beals et al., arXiv:quant-ph/9802049
> **Tags:** #trick #polynomial-method #query-complexity #Boolean-functions #symmetric-functions

## What it does

Reduces the problem of bounding the degree of a symmetric Boolean function from $N$ variables to a single-variable polynomial — dramatically simplifying lower bound proofs.

## The trick

Given a multilinear polynomial $p : \mathbb{R}^N \to \mathbb{R}$ of degree $d$, its **symmetrisation** is:
$$p_{\text{sym}}(X) = \frac{1}{N!} \sum_{\pi \in S_N} p(\pi(X))$$

**Minsky-Papert Lemma:** There exists a single-variable polynomial $q$ of degree $\leq d$ such that $p_{\text{sym}}(X) = q(|X|)$ for all $X \in \{0,1\}^N$.

*Proof:* $p_{\text{sym}}$ is symmetric, so it can be written as a linear combination of the elementary symmetric polynomials $V_j = \sum_{|S|=j} \prod_{i \in S} x_i$. Each $V_j$ evaluates to $\binom{|X|}{j}$ on Boolean inputs, which is a degree-$j$ polynomial in $|X|$.

**Application to PARITY:** Any polynomial $p$ approximating PARITY must have $q(k)$ close to 0 for even $k$ and close to 1 for odd $k$ (or vice versa). So $q(k) - 1/2$ has $N$ sign changes on $\{0, 1, \ldots, N\}$, forcing degree $\geq N$. Therefore $\widetilde{\deg}(\text{PARITY}) = N$, which gives $Q_2(\text{PARITY}) \geq N/2$.

**Application to all symmetric $f$:** Combined with Paturi's (1992) theorem, this gives $\widetilde{\deg}(f) = \Theta(\sqrt{N(N - \Gamma(f))})$ for any non-constant symmetric $f$, where $\Gamma(f)$ measures how close to Hamming weight $N/2$ the function first changes value.

## When to reach for it

- You need to prove a degree lower bound for a symmetric Boolean function.
- You have a polynomial that approximates a symmetric function and want to extract structural constraints.
- You want to reduce a multivariate polynomial question to a univariate one for applying classical approximation theory tools (Chebyshev bounds, Markov brothers' inequality, etc.).

## Complexity

No algorithmic cost — purely a mathematical tool.

## Caveat

Only works for symmetric functions. For non-symmetric functions, symmetrisation destroys the function's structure (e.g., $p = x_0 - x_1$ symmetrises to 0).

## Related notes
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
- [[Adversary Method for Quantum Query Lower Bounds]]
