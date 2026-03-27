
**Source:** Watson, arXiv:2411.04240 (2025), Lemma 3. Builds on AAT24 and earlier Dyson-series techniques.

---

## The trick

Given a randomised simulation channel $\mathcal{E}^N$ (e.g., $N$ repeated qDRIFT steps each of size $s = 1/N$), expand it as a power series in $s$ around $s = 0$:
$$\mathbb{E}\left[\mathcal{E}^{1/s}\right](\rho) = \mathcal{U}_T(\rho) + \sum_{k=1}^{K} c_k \cdot s^k \cdot \Phi_k(\rho) + R_{K+1}(s, \rho)$$
where $\mathcal{U}_T(\rho) = e^{-iHT} \rho e^{iHT}$ is the ideal unitary, $\Phi_k$ are bounded super-operators, and $R_{K+1}$ is a remainder bounded in terms of $s^{K+1}$.

For observable expectations, this gives:
$$\mathbb{E}_{s}[\langle O \rangle] = \langle O \rangle_{\text{exact}} + \sum_{k=1}^{K} c_k' s^k + O(s^{K+1})$$
where $c_k' = \mathrm{tr}(O \Phi_k(\rho))$.

---

## How to derive it

The expansion proceeds by iterating the variation-of-parameters formula. Write the exact channel as $\mathcal{U}_T$ and the qDRIFT channel as a perturbation. For one step, the qDRIFT channel $\mathcal{E}_s$ (one step of size $s$) satisfies:
$$\mathcal{E}_s = \mathcal{U}_s + \mathcal{G}_s$$
where $\mathcal{G}_s$ captures the randomisation error (the difference between the averaged random step and the ideal unitary). For qDRIFT, $\mathbb{E}[\mathcal{E}_s] - \mathcal{U}_s = O(s^2)$ by Campbell's original analysis.

To get the expansion for $N = 1/s$ steps, apply variation-of-parameters (the quantum analogue of the Duhamel principle):
$$\mathcal{E}^N = \mathcal{U}_T + \sum_{j=0}^{N-1} \mathcal{U}_{(N-j-1)s} \circ \mathcal{G}_s \circ \mathcal{E}^j + \text{cross terms}$$

Taking expectations and expanding $\mathcal{G}_s$ in powers of $s$, then collecting terms order-by-order in $s$ gives the coefficients $c_k$ and the bounded super-operators $\Phi_k$.

The remainder bound uses the fact that $\mathcal{E}_s$ is a contraction in appropriate norms (since it's a quantum channel), so the error in truncating at order $K$ is controlled by $s^{K+1} \cdot C_K$ where $C_K$ depends on the operator norm of $H$ and $T$.

---

## Why you need this

Richardson extrapolation (see [[Richardson Extrapolation over Randomized Step Sizes]]) works only if the quantity being extrapolated has a clean power series structure. Without Lemma 3's expansion, you can't guarantee that the Richardson combination cancels the leading errors — you'd just be averaging things without knowing whether the combination is meaningful.

The expansion also identifies *which* terms are being cancelled: the $c_1' s$ term (leading qDRIFT error), then $c_2' s^2$, and so on. This makes the order of extrapolation meaningful.

---

## Generality

The technique applies to any randomised channel that can be written as a small perturbation of a semigroup (here, the ideal unitary semigroup $t \mapsto e^{-iHt}(\cdot)e^{iHt}$). Specifically, it works when:
1. The per-step error $\mathcal{G}_s$ is $O(s^2)$ or smaller in expectation.
2. The channel is contractive so remainder terms don't blow up.
3. The per-step error has itself a power series in $s$ (which it does for qDRIFT, since the random exponential is an analytic function of $s$).

This should extend to other randomised compilers with similar structure, not just qDRIFT.

---

## Contrast with Trotter error expansions

Trotter error expansions (see [[Trotter Commutator-Scaling Bound]]) also produce systematic error terms in powers of the step size. The difference is that Trotter expansions are deterministic and track commutators explicitly. Here, the expansion is in expectation over random choices, and the leading-order term involves the variance of the random sampler rather than commutators.

The mathematical machinery is similar (both use Dyson-series-style arguments), but the coefficients $c_k$ have different physical content: qDRIFT's $c_1'$ tracks $(\lambda T)^2$ (the L1 norm cost) while Trotter's $c_1'$ tracks nested commutators.

---

## Links

- [[Richardson Extrapolation over Randomized Step Sizes]] — uses this expansion as its input
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — source paper (Lemma 3)
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] — the channel being expanded
- [[Diamond-Norm Channel Averaging for Random Compilers]] — related averaging argument
- [[Trotter Commutator-Scaling Bound]] — analogous expansion for deterministic [[Product Formulas]]s
