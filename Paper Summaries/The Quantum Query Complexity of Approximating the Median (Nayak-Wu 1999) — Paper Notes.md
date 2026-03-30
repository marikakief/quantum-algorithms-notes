> **Source:** Ashwin Nayak and Felix Wu, "The quantum query complexity of approximating the median and related statistics", *STOC 1999*
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9804066) · [ACM DL](https://dl.acm.org/doi/10.1145/301250.301349)
> **Tags:** #query-complexity #lower-bounds #polynomial-method #statistics #median #quantum-algorithms

---

## The computational problem

Given a sequence $X = (x_0, \ldots, x_{n-1})$ of $n$ numbers accessed via an oracle, and a parameter $\varepsilon > 0$:

**$\varepsilon$-approximate median:** Find $x_i$ such that the number of elements strictly less than $x_i$, and the number strictly greater than $x_i$, are each less than $(1 + \varepsilon)n/2$.

More generally, the paper considers:
- **$\Delta$-approximate $k$th-smallest element**
- **$\varepsilon$-approximate mean**
- **$\Delta$-approximate count** (for Boolean inputs)
- **$\varepsilon$-approximate relative count**

Classical complexity for finding the median is $\Theta(n)$ comparisons (Blum-Floyd-Pratt-Rivest-Tarjan 1973). The question: can quantum algorithms do better?

## What the paper does

Proves $\Omega(1/\varepsilon)$ quantum query complexity for the $\varepsilon$-approximate median, matching Grover's $\tilde{O}(1/\varepsilon)$ upper bound up to polylogarithmic factors. This gives one of the rare **cubic** quantum-classical separations: quantum $\Theta(1/\varepsilon)$ vs classical $\Theta(n)$, and since $\varepsilon$ can be as small as $1/n$, the separation is $\Theta(n)$ vs $\Theta(n)$ at the exact end but $\Theta(n^{1/3})$ vs $\Theta(n)$ when $\varepsilon = n^{-2/3}$.

Actually, the most interesting case for the cubic separation is finding the exact median ($\Delta = 1$ in the $k$th-smallest formulation): the quantum comparison complexity is $\tilde{\Theta}(\sqrt{k(n-k)})$ while classical is $\Theta(n)$. For $k = n/2$, this gives $\tilde{\Theta}(n)$ quantum vs $\Theta(n)$ classical — no separation. The cubic separation appears for approximate statistics at intermediate precision.

The paper also provides a new algorithm eliminating the domain-size dependence in Grover's earlier algorithm.

## The polynomial degree lower bound

The core technical result is a degree lower bound for polynomials approximating symmetric partial Boolean functions, generalising Paturi (1992).

**Setup:** For $X \in \{0,1\}^n$, let $|X| = \sum x_i$. Define the partial function:

$$f_{\ell, \ell'}(X) = \begin{cases} 1 & \text{if } |X| = \ell \\ 0 & \text{if } |X| = \ell' \end{cases}$$

Let $m \in \{\ell, \ell'\}$ maximise $|n/2 - m|$, and $\Delta_\ell = |\ell - \ell'|$.

**Theorem 1.1:** Any real $n$-variate polynomial approximating $f_{\ell,\ell'}$ to within a constant $c < 1/2$ has degree:

$$d = \Omega\left(\sqrt{n/\Delta_\ell} + \sqrt{m(n-m)/\Delta_\ell}\right)$$

This is tight up to a constant factor (proven by giving a matching upper bound via the approximate counting algorithm of [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Mosca-Tapp]]).

## Proof technique

The proof uses the Markov and Bernstein inequalities for polynomials — if a polynomial has a large derivative at some point in $[-1,1]$, it must have high degree.

**Step 1: Symmetrise and rescale.** Replace $p(x_0, \ldots, x_{n-1})$ by its symmetrisation $p^{\text{sym}}$, then convert to a univariate polynomial $q$ (via Minsky-Papert), and rescale to $\hat{q}$ on $[-1,1]$ where $\hat{q}(x) = q((1+x)n/2)$.

**Step 2: Find a large derivative.** By the Mean Value Theorem, since $\hat{q}(a_\ell) \geq 2/3$ and $\hat{q}(a_{\ell'}) \leq 1/3$, there exists $a \in [a_{\ell'}, a_\ell]$ where:

$$\hat{q}'(a) \geq \frac{1/3}{a_\ell - a_{\ell'}} = \frac{n}{6\Delta_\ell}$$

**Step 3a: Markov inequality.** If $\|\hat{q}\| < 2$, then $d^2 \geq \hat{q}'(a)/\|\hat{q}\| \geq n/(12\Delta_\ell)$, giving $d = \Omega(\sqrt{n/\Delta_\ell})$. If $\|\hat{q}\| \geq 2$, the polynomial must achieve this large value close to some point $a_i$ where $|\hat{q}| \leq 4/3$, again forcing high degree.

**Step 3b: Bernstein inequality.** The Bernstein inequality relates $\sqrt{1-a^2} \cdot |\hat{q}'(a)|$ to $d \cdot \|\hat{q}\|$. Since $a \in [a_{\ell'}, a_\ell]$, we have $1 - a^2 \geq 4m(n-m)/n^2$, giving $d = \Omega(\sqrt{m(n-m)/\Delta_\ell})$.

When $\|\hat{q}\| \geq 2$ and the norm is achieved near the boundary of $[-1,1]$, a damping argument multiplies $\hat{q}$ by $(1-x^2)^{d_1}$ or transforms to a trigonometric polynomial and applies the trigonometric Bernstein inequality.

## Reduction to statistics problems

The polynomial bound feeds into query lower bounds via the Beals-Buhrman-Cleve-Mosca-de Wolf lemma: a $T$-query quantum algorithm's acceptance probability on Boolean input is a degree-$2T$ multilinear polynomial.

**For the $k$th-smallest element:** A $\Delta$-approximate $k$th-smallest algorithm computes $f_{n-k+\Delta, n-k-\Delta}$, giving:

$$\text{Queries} = \Omega\left(\sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta}\right)$$

**For the median** ($k = n/2$, $\Delta = \varepsilon n/2$): $\Omega(1/\varepsilon)$ queries.

## The quantum-classical separation

| Problem | Classical | Quantum | Separation |
|---|---|---|---|
| Exact median ($k = n/2$, $\Delta = 1$) | $\Theta(n)$ | $\tilde{\Theta}(n)$ | None |
| Exact $k$th-smallest ($\Delta = 1$) | $\Theta(n)$ | $\tilde{\Theta}(\sqrt{k(n-k)})$ | Up to $\sqrt{n}$ |
| Exact minimum ($k = 1$, $\Delta = 1$) | $\Theta(n)$ | $O(\sqrt{n})$ | Quadratic |
| $\varepsilon$-approximate median | $\Theta(n)$ | $\tilde{\Theta}(1/\varepsilon)$ | Up to $n$ (when $\varepsilon$ constant) |
| $\Delta$-approximate count | $\Theta(n)$ | $\Theta(\sqrt{n/\Delta} + \sqrt{t(n-t)/\Delta})$ | Depends on $t, \Delta$ |

The cubic separation arises in the comparison model when $\varepsilon = \Theta(1)$: quantum needs $O(1)$ queries while classical needs $\Theta(n)$. More precisely, setting $\varepsilon = n^{-2/3}$ gives quantum $\tilde{O}(n^{2/3})$ vs classical $\Theta(n)$ — a cube-root separation.

## The algorithm

The paper gives an $O(\frac{1}{\varepsilon} \log \frac{1}{\varepsilon} \log \log \frac{1}{\varepsilon})$-query algorithm for $\varepsilon$-approximate median, improving over Grover's earlier algorithm by eliminating dependence on the domain size $M$.

The algorithm performs quantum binary search using:
- **Sampler $S(i,j)$:** Uses the [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp]] generalised search to sample uniformly from elements in a given range; $O(\sqrt{n/\Delta})$ queries per call
- **Distinguisher $K'(i)$:** Uses [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Mosca-Tapp]] approximate counting to determine whether $x_i$ has rank close to $k$; $O(\sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta})$ queries per call
- **Binary search wrapper:** Expected $O(\log N)$ stages, each calling $S$ and $K'$

## Key results

| Result | Bound |
|---|---|
| Polynomial degree for $f_{\ell,\ell'}$ | $\Omega(\sqrt{n/\Delta_\ell} + \sqrt{m(n-m)/\Delta_\ell})$ — tight |
| $\varepsilon$-approximate median lower bound | $\Omega(1/\varepsilon)$ |
| $\varepsilon$-approximate median algorithm | $O(\frac{1}{\varepsilon} \log \frac{1}{\varepsilon} \log \log \frac{1}{\varepsilon})$ |
| $\Delta$-approximate $k$th-smallest lower bound | $\Omega(\sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta})$ |
| $\Delta$-approximate $k$th-smallest algorithm | $O(N \log N \log \log N)$ where $N = \sqrt{n/\Delta} + \sqrt{k(n-k)/\Delta}$ |
| $\Delta$-approximate count (instance-optimal) | $\Theta(\sqrt{n/\Delta} + \sqrt{t(n-t)/\Delta})$ on inputs with $t$ ones |

## Comparison with prior work

| Algorithm | Query complexity | Domain dependence |
|---|---|---|
| Classical (Blum et al. 1973) | $\Theta(n)$ | None |
| Grover (1996) | $\tilde{O}(1/\varepsilon)$ | $O(\log M)$ factor |
| Dürr-Høyer (1996, minimum only) | $O(\sqrt{n})$ | None |
| **This paper** | $\tilde{O}(1/\varepsilon)$ | None |

## Limits / caveats

- The polylogarithmic gap between upper and lower bounds ($\log(1/\varepsilon) \cdot \log\log(1/\varepsilon)$) remains open. The authors conjecture the lower bound is tight and the algorithm can be improved.
- The polynomial method used here cannot yield better bounds for these problems (proven by the matching polynomial upper bound in Corollary 1.4).
- All bounds hold in the comparison tree model as well (4 oracle queries simulate one comparison).
- The "cubic separation" framing requires care: it's cubic in the comparison model at specific parameter settings, not a blanket statement.

## Reusable ideas

1. [[Paturi-Nayak-Wu Degree Bound for Partial Symmetric Functions]] — the polynomial degree lower bound for symmetric partial Boolean functions, combining Markov/Bernstein inequalities with damping arguments; applies to any problem reducible to distinguishing Hamming weight slices
2. [[Quantum Binary Search via Approximate Counting]] — using Brassard-Høyer-Mosca-Tapp approximate counting as a subroutine inside a random-pivot binary search, achieving near-optimal query complexity for order statistics

## References within this paper

- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — the hybrid argument giving $\Omega(\sqrt{N})$ for search
- Beals, Buhrman, Cleve, Mosca, de Wolf, "Quantum lower bounds by polynomials", FOCS 1998 — the polynomial method
- Paturi, "On the degree of polynomials that approximate symmetric Boolean functions", STOC 1992 — the degree bound this paper generalises
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr-Høyer (1996)]] — minimum finding algorithm that inspired the binary search approach
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] — generalised search used as sampling subroutine
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] — approximate counting used as distinguisher subroutine
- Grover, "A fast quantum mechanical algorithm for estimating the median", 1996 — the earlier algorithm this paper improves
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes|Bravyi-Harrow-Hassidim (2011)]] — later work on quantum property testing with related $\Theta(N^{1/3})$ bounds

## Cross-links

### Paper notes
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — the hybrid/adversary lower bound methods
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] — the minimum-finding algorithm
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] — the search framework used here
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]] — approximate counting subroutine
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]] — related $O(N^{1/3})$ bounds for distribution testing
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]] — the adversary method cannot prove $\Omega(1/\varepsilon)$ for approximate median (certificate barrier applies)
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — another polynomial method application

### Trick cards
- [[Paturi-Nayak-Wu Degree Bound for Partial Symmetric Functions]]
- [[Quantum Binary Search via Approximate Counting]]
- [[Amplitude Estimation via Phase Estimation on Q]]
