# Large Deviation Bound for Random Unitaries

> **Source:** Hayden, Leung, Shor & Winter, arXiv:quant-ph/0307104
> **Tags:** #trick #concentration #Haar-random #large-deviation #randomization

## What it does

Provides an exponential tail bound for the empirical average of $\operatorname{Tr}(U\varphi U^\dagger P)$ over i.i.d. Haar-random unitaries, where $\varphi$ is a pure state and $P$ is a rank-$p$ projector.

## The trick

For i.i.d. Haar-distributed unitaries $U_1, \ldots, U_n \in U(d)$, pure state $\varphi$, and rank-$p$ projector $P$:

$$\Pr\left[\left|\frac{1}{n}\sum_{j=1}^n \operatorname{Tr}(U_j \varphi U_j^\dagger P) - \frac{p}{d}\right| \geq \frac{\epsilon p}{d}\right] \leq 2\exp(-Cnp\epsilon^2)$$

with $C \geq (6\ln 2)^{-1}$.

The proof bootstraps from Gaussian random vectors. Write $|g\rangle = \sum_i g_i |i\rangle$ with i.i.d. $g_i \sim \mathcal{N}_{\mathbb{C}}(0,1)$. The distribution of $|g\rangle / \|g\|$ is Haar-uniform on the unit sphere. By Jensen's inequality (convexity of $\exp$):

$$\mathbb{E}_g \exp\left(\frac{\lambda}{d}\sum_{i=1}^p |g_i|^2\right) \geq \mathbb{E}_U \exp\left(\lambda \operatorname{Tr}(U\varphi U^\dagger P)\right)$$

This means the rate function $\Lambda^*_U(x) \geq \Lambda^*_p(x)$, and the latter reduces to $p$ independent $|g_i|^2$ variables. The rate function for $|g_i|^2$ is $\Lambda^*_g(1+\epsilon) \geq C\epsilon^2$, giving the bound via Cramér's theorem.

## When to reach for it

- Proving that random unitary constructions work: [[Approximate Randomization with Few Unitaries|approximate randomization]], approximate $t$-designs, quantum data hiding.
- Any argument requiring concentration of quadratic forms $\operatorname{Tr}(U\varphi U^\dagger P)$ around their mean $p/d$.
- Combined with an $\epsilon$-net argument ($|M| \leq (5/\epsilon)^{2d}$), gives uniform concentration over all states simultaneously.

## Complexity

Not applicable — this is an analytical tool, not a computational procedure. The bound is tight up to the constant $C$ (which could be improved using the exact distribution computed by Życzkowski & Sommers).

## Caveat

The bound degrades for small $p$: the exponent is $Cnp\epsilon^2$, so projectors of rank $p = O(1)$ require $n = \Omega(1/\epsilon^2)$ regardless of $d$. The Gaussian bootstrapping only gives a lower bound on the true rate function; the exact rate function would yield tighter constants. The result is for i.i.d. Haar-random unitaries; structured ensembles (Clifford, approximate designs) require different concentration arguments.

## Related notes
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]] — primary application
- [[Approximate Randomization with Few Unitaries]]
- [[Submatrix-of-Haar-Unitary to Gaussian Approximation]]
