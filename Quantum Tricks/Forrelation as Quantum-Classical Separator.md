# Forrelation as Quantum-Classical Separator

> **Source:** Aaronson, arXiv:0910.4698 (2010); Aaronson-Ambainis, arXiv:1411.5729 (2015)
> **Tags:** #trick #BQP #PH #query-complexity #Fourier-analysis #oracle-separation

## What it does

Provides a problem — computing the forrelation between two Boolean functions — that achieves the maximum possible quantum-classical query separation: $O(1)$ quantum queries vs. $\tilde{\Omega}(\sqrt{N})$ classical queries.

## The trick

The **forrelation value** between $f, g : \{0,1\}^n \to \{-1,+1\}$ is:

$$\Phi_{f,g} = \frac{1}{N^{3/2}} \sum_{x,y \in \{0,1\}^n} f(x)(-1)^{x \cdot y}g(y) = \frac{1}{\sqrt{N}}\sum_y \hat{f}(y)g(y)$$

This measures "how correlated is $g$ with the Fourier transform of $f$?"

**Quantum:** The $H$-$O_f$-$H$-$O_g$-$H$-measure circuit produces outcome $|0^n\rangle$ with probability $\Phi_{f,g}^2$. One query to each oracle suffices.

**Classical:** Any bounded-error algorithm needs $\tilde{\Omega}(\sqrt{N})$ queries. This matches the general simulation upper bound ($t$ quantum queries → $O(N^{1-1/(2t)})$ classical queries), so the gap is optimal.

The promise version (forrelated vs. uniform pairs) is the decision problem [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes|Fourier Checking]]. The $k$-fold generalisation with $k = \text{poly}(n)$ functions is BQP-complete.

## When to reach for it

- As the canonical example of a problem separating BQP from PH (relative to an oracle)
- When you need a "maximally quantum" problem — Raz-Tal (2019) showed Forrelation is the problem that maximally separates quantum from classical in query complexity
- As a template for constructing new quantum-classical separations: look for problems where the quantum algorithm computes a "global correlation" (involving all input bits) that classical algorithms can't access without reading most of the input
- When teaching: the Forrelation circuit is the simplest non-trivial quantum algorithm that provably outperforms the entire polynomial hierarchy

## Complexity

Quantum: $O(1)$ queries, $O(n)$ gates. Classical: $\tilde{\Omega}(\sqrt{N})$ queries, where $N = 2^n$.

## Caveat

The separation is for *partial* (promise) Boolean functions. For total functions, no superpolynomial quantum-classical separation is possible (Aaronson-Ambainis 2015 simulation theorem). Also, the separation is in query complexity — translating to circuit complexity separations remains open.

## Related notes
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]] — introduced Forrelation
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — proved the $\tilde{\Omega}(\sqrt{N})$ lower bound and optimality
- [[Gaussian Lifting for Query Lower Bounds]]
- [[Near-Orthogonality of Standard-Fourier Basis Pairs]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
