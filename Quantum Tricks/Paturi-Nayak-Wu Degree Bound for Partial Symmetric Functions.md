# Paturi-Nayak-Wu Degree Bound for Partial Symmetric Functions

> **Source:** Nayak and Wu, arXiv:quant-ph/9804066 (1999); extends Paturi (1992)
> **Tags:** #trick #polynomial-method #degree-bound #lower-bounds #query-complexity #symmetric-functions

## What it does
Gives a tight degree lower bound for real polynomials that approximate symmetric partial Boolean functions (functions defined only on specific Hamming weight slices), which converts to quantum query lower bounds via the Beals-Buhrman-Cleve-Mosca-de Wolf polynomial lemma.

## The trick

Define $f_{\ell, \ell'} : \{0,1\}^n \to \{0,1\}$ as the partial function that is 1 on Hamming weight $\ell$ and 0 on weight $\ell'$. Let $\Delta_\ell = |\ell - \ell'|$ and let $m \in \{\ell, \ell'\}$ maximise $|n/2 - m|$.

**Theorem:** Any real $n$-variate polynomial approximating $f_{\ell,\ell'}$ to within constant $c < 1/2$ has degree:

$$d = \Omega\left(\sqrt{n/\Delta_\ell} + \sqrt{m(n-m)/\Delta_\ell}\right)$$

**Proof sketch:**
1. Symmetrise and convert to univariate polynomial $\hat{q}$ on $[-1,1]$
2. By Mean Value Theorem, $\hat{q}'(a) \geq n/(6\Delta_\ell)$ for some $a$ between the two weight slices
3. **Markov bound** $|p'(x)| \leq d^2 \|p\|$ gives $d = \Omega(\sqrt{n/\Delta_\ell})$
4. **Bernstein bound** $\sqrt{1-x^2}|p'(x)| \leq d\|p\|$ gives $d = \Omega(\sqrt{m(n-m)/\Delta_\ell})$ using $1 - a^2 \geq 4m(n-m)/n^2$
5. When $\|p\| \geq 2$, damping by $(1-x^2)^{d_1}$ or switching to trigonometric polynomials handles the large-norm case

This subsumes Paturi's (1992) bound for total symmetric functions and is tight (matching algorithms via approximate counting).

## When to reach for it

- Proving quantum query lower bounds for any problem that reduces to distinguishing two Hamming weight classes: approximate counting, approximate median, $k$th-smallest element, approximate mean
- Any setting where the polynomial method applies and the function has a symmetric structure (even partial symmetry suffices after symmetrisation)
- Understanding the limitations of the polynomial method: this bound is tight, so it identifies the maximum power of the polynomial approach for these problems

## Complexity
The degree bound translates to query complexity via $T \geq d/2$. For approximate median ($k = n/2$, $\Delta = \varepsilon n$): $\Omega(1/\varepsilon)$ queries. For exact $k$th-smallest ($\Delta = 1$): $\Omega(\sqrt{k(n-k)})$ queries.

## Caveat
The bound applies only to symmetric partial functions (or problems reducible to them). For non-symmetric partial functions, different polynomial techniques (or the adversary method) may be needed. The damping argument for the large-norm case at the boundary of $[-1,1]$ is the most technically involved part and requires careful choice of damping parameters.

## Related notes
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]]
- [[Quantum Binary Search via Approximate Counting]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]] — the adversary method cannot match this bound for approximate statistics (certificate barrier)
