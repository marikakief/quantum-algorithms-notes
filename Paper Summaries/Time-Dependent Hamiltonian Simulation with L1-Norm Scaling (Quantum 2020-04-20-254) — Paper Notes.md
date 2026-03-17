> **Source:** Dominic W. Berry, Andrew M. Childs, Yuan Su, Xin Wang, and Nathan Wiebe, *Time-dependent Hamiltonian simulation with L1-norm scaling*, arXiv:1906.07115, Quantum **4**, 254 (2020)  
> **Links:** [arXiv](https://arxiv.org/abs/1906.07115) · [Quantum](https://doi.org/10.22331/q-2020-04-20-254)  
> **Tags:** #hamiltonian-simulation #time-dependent #dyson #qdrift #LCU

---

## What the paper does

Fixes a real inefficiency in time-dependent simulation complexity: prior bounds scaled with $t \cdot \max_\tau \|H(\tau)\|$, paying for the peak norm even when the Hamiltonian is large only briefly.

The paper shows that the right parameter is the **integrated (L1) norm**. For a $d$-sparse Hamiltonian in the sparse-matrix oracle model, this is:

$$
\|H\|_{\max,1} := \int_0^t d\tau\, \|H(\tau)\|_{\max},
$$

where $\|H(\tau)\|_{\max}$ is the largest entry of $H(\tau)$ in absolute value. (In the LCU model with $H(\tau) = \sum_l \alpha_l(\tau) H_l$, the analogous quantity is $\|\alpha\|_{1,1} = \int_0^t \sum_l \alpha_l(\tau)\,d\tau$.)

Two algorithms achieve this scaling.

## Algorithm 1: Continuous qDRIFT

A randomized generalization of Campbell's qDRIFT to time-dependent Hamiltonians. At each step, sample a time $\tau \sim p(\tau) \propto \|H(\tau)\|_{\max}$ and a term from the decomposition at that time, then apply a scaled short evolution.

The result is a **mixed-unitary channel** that approximates the true evolution. Error is bounded (Theorem 7') as:

$$
\left\|E(t,0) - \prod_{j} \mathcal{U}(t_{j+1},t_j)\right\|_\diamond \le \frac{4\|H\|_{\max,1}^2}{r},
$$

where the interval is split into $r$ equal-integrated-norm segments. To achieve error $\varepsilon$, take $r \ge 4\|H\|_{\max,1}^2/\varepsilon$.

**Complexity (Corollary 9):**

$$
O\!\left(\frac{d^4\,\|H\|_{\max,1}^2}{\varepsilon}\right) \text{ queries},\quad \widetilde{O}\!\left(\frac{d^4\,\|H\|_{\max,1}^2\,n}{\varepsilon}\right) \text{ gates}.
$$

Quadratic in the integrated norm. Simple and potentially near-term-friendly, but asymptotically weaker than Algorithm 2.

## Algorithm 2: Rescaled Dyson series

The key idea is a **time-reparameterization principle**. Define the cumulative norm

$$
s = f(t) := \int_0^t d\tau\,\|H(\tau)\|_{\max}
$$

and the rescaled Hamiltonian

$$
\tilde{H}(s) := \frac{H(f^{-1}(s))}{\|H(f^{-1}(s))\|_{\max}}.
$$

By construction $\|\tilde{H}(s)\|_{\max} = 1$ everywhere, so the rescaled system has flat norm profile. The original evolution satisfies $E(t,0) = \mathcal{T}\exp(-i\int_0^S \tilde{H}(s)\,ds)$ where $S = \|H\|_{\max,1}$.

Applying the truncated-Dyson-series algorithm (Kieferová–Scherer–Berry, or the LCU version of Berry et al. 2018) to $\tilde{H}(s)$ on the rescaled interval $[0, S]$ gives:

**Complexity (Theorem 10, sparse model):**

$$
O\!\left(d\,\|H\|_{\max,1}\cdot\frac{\log(d\,\|H\|_{\max,1}/\varepsilon)}{\log\log(d\,\|H\|_{\max,1}/\varepsilon)}\right) \text{ queries}.
$$

Nearly linear in the integrated norm (up to polylog factors). Additional oracle access needed: $O_{\rm var}$ to compute $f^{-1}(s)$ and $O_{\rm norm}$ for $\|H(\tau)\|_{\max}$.

## Comparison

| Method | Error metric | Complexity scaling |
|---|---|---|
| Worst-case (older) | spectral/max-norm | $t \cdot \max_\tau \|H(\tau)\|_{\max}$ |
| Continuous qDRIFT (this paper) | diamond norm | $\|H\|_{\max,1}^2/\varepsilon$ (quadratic in integrated norm) |
| Rescaled Dyson (this paper) | spectral norm | $\|H\|_{\max,1}\cdot\widetilde{O}(1)$ (near-linear) |

The difference between quadratic and near-linear is significant when $\|H\|_{\max,1} \gg \varepsilon$, which is the relevant regime for precision simulation.

## Application: chemical scattering

The paper identifies semi-classical scattering of molecules as a natural target. As nuclei approach and recede, $\|H(t)\|$ varies by orders of magnitude. The L1-norm improvement can be substantial here precisely because the Hamiltonian is large only during the brief near-collision phase.

## Caveats

- The rescaled algorithm requires oracle access to the cumulative norm function $f(t)$ (or an efficiently computable upper bound), plus the ability to compute $f^{-1}$.
- The qDRIFT algorithm is simpler but quadratically worse in the integrated norm. It may be preferable when circuit overhead is the bottleneck.
- If $\|H(t)\|$ is approximately constant in time, the improvement over older bounds is minimal.

## References within this paper

- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer & Berry (2019)]] — Dyson series simulation that this paper builds on
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — qDRIFT; this paper extends the idea to continuous-time/time-dependent settings
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2019)]] — interaction-picture Dyson simulation
- Berry, Childs, Cleve, Kothari & Somma (2015) — Taylor-series LCU (comparison point)
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP optimal scaling

---

## Cross-links

- [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]]
- [[Integrated-Norm Equal-Segment Partitioning]]
- [[Time-Reparameterization for Norm-Flattened Dyson Simulation]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
