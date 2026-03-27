
> **Source:** Montanaro, arXiv:1504.06987; classical technique from Štefankovič-Vempala-Vigoda (2009)
> **Tags:** #trick #partition-function #monte-carlo #cooling-schedule #statistical-physics

## What it does
Reduces the problem of estimating a partition function $Z(\beta)$ to estimating $\ell$ ratios of consecutive partition functions, each of which is a bounded-relative-variance mean estimation problem.

## The trick

1. **Telescoping product.** Choose inverse temperatures $0 = \beta_0 < \beta_1 < \cdots < \beta_\ell = \infty$:
$$Z(\beta_\ell) = Z(\beta_0) \prod_{i=0}^{\ell-1} \frac{Z(\beta_{i+1})}{Z(\beta_i)}$$
$Z(\beta_0) = |\Omega|$ is known.

2. **Each ratio as a mean.** Define $Y_i(x) = e^{-(\beta_{i+1}-\beta_i)H(x)}$ sampled under the Gibbs distribution $\pi_i$. Then $\mathbb{E}_{\pi_i}[Y_i] = Z(\beta_{i+1})/Z(\beta_i)$.

3. **Chebyshev cooling schedule.** Choose the $\beta_i$ so that $\mathbb{E}[Y_i^2]/\mathbb{E}[Y_i]^2 \leq B = O(1)$ for all $i$. This bounds the relative variance, making each ratio easy to estimate. A schedule of length $\ell = \tilde{O}(\sqrt{\log |\Omega|})$ always exists.

4. **Estimate each ratio.** Use [[Quantum Mean Estimation via Amplitude Estimation|quantum mean estimation]] for $\tilde{O}(B/\epsilon)$ cost per ratio (relative error). Classically: $O(B/\epsilon^2)$.

5. **Multiply.** $\tilde{Z} = Z(\beta_0) \prod_i \tilde{\alpha}_i$ approximates $Z(\beta_\ell)$ to relative error $\epsilon$.

**Quantum total cost:** $\tilde{O}(\ell^2 \sqrt{\tau}/\epsilon)$ quantum walk steps, where $\tau$ is the Markov chain relaxation time.

**Classical total cost:** $\tilde{O}(\ell^2 \tau/\epsilon^2)$ — quadratically worse in both $\tau$ and $\epsilon$.

## When to reach for it
- Approximating partition functions in statistical physics (Ising, Potts, monomer-dimer)
- Counting combinatorial structures (colourings, matchings) via the partition function framework
- Any problem reducible to a product of ratios of normalisation constants

## Complexity
$\tilde{O}((\log |\Omega|) \cdot \sqrt{\tau}/\epsilon)$ quantum walk steps for the full partition function estimation (with Chebyshev schedule).

## Caveat
The Chebyshev cooling schedule itself is computed classically in $\tilde{O}(\log |\Omega| \cdot \tau)$ time — this step isn't sped up quantumly (no-cloning prevents reusing quantum states as warm starts). When $\tau$ is the bottleneck, the classical schedule computation can dominate. Also requires the Markov chains to mix rapidly — if $\tau$ is exponential, no help.

## Related notes
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
- [[Quantized Bipartite Walk Construction]] — Szegedy walk used for sampling from Gibbs distributions
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
