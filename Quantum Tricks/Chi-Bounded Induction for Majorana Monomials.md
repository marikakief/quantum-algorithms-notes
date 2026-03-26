# Chi-Bounded Induction for Majorana Monomials

> **Source:** King, Gosset, Kothari, Babbush, arXiv:2404.19211
> **Tags:** #trick #chi-boundedness #graph-coloring #fermionic #Majorana #shadow-tomography #induction

## What it does

Establishes that induced subgraphs of the commutation graph of $k$-body fermionic operators have fractional chromatic number bounded by a polynomial in the clique number — with no dependence on the system size $n$. This is the technical engine behind triply efficient fermionic shadow tomography.

## The trick

The commutation graph $G(\mathcal{F}^{(n)}_k)$ of degree-$2k$ Majorana monomials has the property that its chromatic/fractional chromatic number is polynomially bounded by its clique number — it is **chi-bounded** with a polynomial binding function.

The proof proceeds by induction on the Majorana monomial degree $r$, establishing fractional colorings of size $f_r(\omega)$ for induced subgraphs with clique number $\omega$.

**Base case ($r = 2$, i.e., $k = 1$):** 1-body operators $ic_a c_b$ define a line graph of a bipartite-like structure on Majorana modes. Edge coloring via Misra-Gries gives $f_2(\omega) = \omega + 1$.

**Odd degree step ($r \geq 3$, odd):** Given vertex set $V \subseteq M^{(n)}_r$, find a maximal anticommuting set of size $L \leq \omega$. Let $I$ be the union of their Majorana supports ($|I| \leq r\omega$). Partition $V$ by the first Majorana index each operator shares with $I$. Each partition class has operators sharing a common Majorana factor — strip it off to reduce to degree $r-1$. Sample the fractional coloring: pick a partition uniformly, then color within it. Size: $f_r(\omega) = r\omega \cdot f_{r-1}(\omega)$.

**Even degree step ($r \geq 4$, even):** For each Majorana index $i \in [2n]$, collect all operators in $V$ containing $c_i$. Strip $c_i$ to reduce to degree $r-1$. Sample independent sets from all $2n$ sub-problems and intersect: an operator is selected iff it's selected in every sub-problem corresponding to its Majorana support. The intersection remains an independent set because operators sharing a Majorana factor commute iff their reduced versions commute. Size: $f_r(\omega) = (f_{r-1}(\omega))^r$.

The resulting polynomials for $k$-body operators ($r = 2k$):

| $k$ | $p_k(\omega) = f_{2k}(\omega)$ | Sample complexity scaling in $\epsilon$ |
|---|---|---|
| 1 | $\omega + 1$ | $\epsilon^{-4}$ |
| 2 | $O(\omega^8)$ | $\epsilon^{-18}$ |
| 3 | $O(\omega^{54})$ | $\epsilon^{-110}$ |
| General | $\leq (2k\omega)^{(2k)^{k+1}}$ | $\epsilon^{-O((2k)^{k+1})}$ |

## When to reach for it

- Learning $k$-body fermionic operators (RDMs) with two-copy measurements and sample complexity independent of system size $n$.
- Any shadow tomography problem where the observable set has commutation structure amenable to recursive degree reduction.
- Understanding the gap between single-copy ($\Omega(n^k)$ samples) and two-copy ($\mathrm{poly}(1/\epsilon) \cdot \log n$ samples) fermionic tomography.

## Complexity

The fractional coloring can be sampled in $\mathrm{poly}(n)$ time for any fixed $k = O(1)$: each inductive step involves finding maximal anticommuting sets (greedy, $\mathrm{poly}(n)$) and calling the lower-degree subroutine $O(n)$ times.

## Caveat

The polynomial degree explodes with $k$. For $k = 2$, you get $\omega^8 \sim \epsilon^{-16}$; for $k = 3$, $\omega^{54} \sim \epsilon^{-108}$. This makes the protocol impractical for $k \geq 2$ unless $n$ is astronomically large relative to $1/\epsilon$. For the 2-RDM and higher, [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|matchgate shadows]] with $O(n^k \log n / \epsilon^2)$ single-copy scaling are likely more practical for realistic $n$.

Improving these polynomials — or showing they're tight — is an open problem.

## Related notes
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
- [[Fractional Coloring of Commutation Graphs]]
- [[Clique Bound from Uncertainty Principle]]
- [[Matchgate 3-Design for Classical Shadows]]
- [[Bravyi-Kitaev Transformation]]
