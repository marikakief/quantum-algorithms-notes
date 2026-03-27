> **Source:** Dominic W. Berry, Andrew M. Childs, Yuan Su, Xin Wang, and Nathan Wiebe, *Time-dependent [[Hamiltonian simulation]] with $L^1$-norm scaling*, arXiv:1906.07115, Quantum **4**, 254 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1906.07115) · [Quantum](https://doi.org/10.22331/q-2020-04-20-254)
> **Tags:** #hamiltonian-simulation #time-dependent #dyson #qdrift #LCU #L1-norm

---

## The computational problem

Given a time-dependent Hamiltonian $H(\tau)$ for $\tau \in [0,t]$, approximate the time-ordered evolution operator

$$
\mathcal{T}\exp\!\left(-i\int_0^t d\tau\, H(\tau)\right)
$$

within error $\varepsilon$ (spectral norm or diamond norm). Two input models:
- **Sparse matrix (SM):** $H(\tau)$ is $d$-sparse, accessed via location and value oracles $\mathcal{O}_{\mathrm{loc}}, \mathcal{O}_{\mathrm{val}}$. Cost measured in queries + gates.
- **[[Linear Combination of Unitaries (LCU)|LCU]]:** $H(\tau) = \sum_{l=1}^L \alpha_l(\tau) H_l$ with $H_l$ unitary-Hermitian, coefficients classically computable. Cost measured in gates.

The question: can the gate/query complexity scale with the **integrated** norm $\int_0^t \|H(\tau)\| \, d\tau$ rather than the product $t \cdot \max_\tau \|H(\tau)\|$?

## What the paper does

Answers yes, via two new algorithms. The key observation: prior time-dependent simulation bounds all paid for the **peak** norm of $H$ across the full interval, even when $\|H(\tau)\|$ varies wildly. This is wasteful — the simulation difficulty at time $\tau$ should depend only on $\|H(\tau)\|$ at that instant.

For a $d$-sparse Hamiltonian, the relevant parameter becomes

$$
\|H\|_{\max,1} := \int_0^t d\tau\, \|H(\tau)\|_{\max}
$$

instead of $t \cdot \|H\|_{\max,\infty}$. In the [[Linear Combination of Unitaries (LCU)|LCU]] model, replace $\|\alpha\|_{1,\infty} \cdot t$ with $\|\alpha\|_{1,1} = \int_0^t \sum_l \alpha_l(\tau)\,d\tau$.

Two algorithms achieve this:
1. **Continuous qDRIFT** — simple, randomized, near-term-friendly, but quadratic in the integrated norm.
2. **Rescaled Dyson series** — near-optimal (linear in integrated norm up to polylogs), requires coherent [[Linear Combination of Unitaries (LCU)|LCU]] machinery.

My assessment: this paper is a clean, important contribution. The time-reparameterization idea is elegant and general — it should apply to any simulation algorithm whose complexity scales with $t \cdot \|H\|$. The continuous [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] is a nice bonus but the rescaling principle is the real lasting idea.

## Algorithm 1: Continuous qDRIFT

Generalizes Campbell's [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|qDRIFT]] to the time-dependent setting. The construction:

1. Partition $[0,t]$ into $r$ segments with equal integrated norm (see [[Integrated-Norm Equal-Segment Partitioning]])
2. On each segment $[t_j, t_{j+1}]$, sample a time $\tau$ from $p(\tau) \propto \|H(\tau)\|_{\max}$
3. Sample a term $H_l(\tau)$ from the [[Linear Combination of Unitaries (LCU)|LCU]] decomposition at time $\tau$
4. Apply a short evolution by that term, scaled by the integrated norm

This produces a **mixed-unitary channel** approximating the true evolution.

**Universality (Theorem 5):** Any time-independent Hamiltonian that can be simulated by the original [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] can also be simulated by continuous qDRIFT with the same complexity. The paper shows this by constructing a time-dependent Hamiltonian whose continuous qDRIFT simulation reproduces the original qDRIFT channel. This also gives a factor-of-2 improvement in the constant for the original qDRIFT bound.

## Algorithm 2: Rescaled Dyson series

The main result. Uses a [[Time-Reparameterization for Norm-Flattened Dyson Simulation|time reparameterization]] to flatten the norm profile.

**The rescaling principle (Proposition 8).** Define the cumulative norm function

$$
s = f(t) := \int_0^t d\tau\,\|H(\tau)\|_{\max}
$$

and the rescaled Hamiltonian

$$
\tilde{H}(s) := \frac{H(f^{-1}(s))}{\|H(f^{-1}(s))\|_{\max}}.
$$

Then $\|\tilde{H}(s)\|_{\max} = 1$ for all $s$, and the original evolution is

$$
E(t,0) = \mathcal{T}\exp\!\left(-i\int_0^S \tilde{H}(s)\,ds\right), \quad S = \|H\|_{\max,1}.
$$

The inequality $\int_0^t \|H\| \le t \max \|H\|$ is now saturated (holds with equality). Any simulation algorithm applied to $\tilde{H}$ on $[0,S]$ automatically achieves $L^1$-norm scaling.

Apply the [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|truncated Dyson-series algorithm]] to $\tilde{H}$ to get:

## Key results

**Theorem 4 (Fractional-query, SM).** A $d$-sparse time-dependent $H(\tau)$ on $n$ qubits can be simulated to error $\varepsilon$ with

$$
O\!\left(d^2 \|H\|_{\max,1} \cdot \frac{\log(d^2 \|H\|_{\max,\infty} t/\varepsilon)}{\log\log(d^2 \|H\|_{\max,\infty} t/\varepsilon)}\right)
$$

queries. This is the first $L^1$-norm query complexity result. (Gate complexity not explicitly given — only query complexity achieved here.)

**Theorem 5 (Universality of continuous qDRIFT).** Continuous qDRIFT can simulate any Hamiltonian that the original [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] can, with the same asymptotic complexity. The constant improves by a factor of 2 via direct evaluation of $\frac{d}{d\lambda}\|e^{i\lambda H_l t} \rho e^{-i\lambda H_l t}\|$.

**Corollary 9 (Continuous qDRIFT, SM gate complexity).**

$$
\widetilde{O}\!\left(\frac{(d^2 \|H\|_{\max,1})^2 \, n}{\varepsilon}\right) \text{ gates.}
$$

Quadratic in integrated norm. Simple but asymptotically weaker.

**Theorem 10 (Rescaled Dyson, SM).**

$$
O\!\left(d\,\|H\|_{\max,1}\cdot\frac{\log(d\,\|H\|_{\max,1}/\varepsilon)}{\log\log(d\,\|H\|_{\max,1}/\varepsilon)}\right) \text{ queries.}
$$

$$
\widetilde{O}(d\,\|H\|_{\max,1}\, n) \text{ gates.}
$$

Near-linear in integrated norm. Requires additional oracles $O_{\rm var}$ (inverse cumulative norm) and $O_{\rm norm}$ (instantaneous norm).

**Theorem 11 (Rescaled Dyson, LCU).**

$$
\widetilde{O}(\|\alpha\|_{\infty,1} \, L^2 \, g_c) \text{ gates.}
$$

where $g_c$ is the cost of controlled-$H_l$.

## Comparison with prior work

| Algorithm | SM gate complexity | LCU gate complexity | Norm scaling |
|---|---|---|---|
| Monte Carlo (Poulin et al.) | $\widetilde{O}((d^2 \|H\|_{\max,\infty} t)^2 n/\varepsilon)$ | $O((\|\alpha\|_{1,\infty} t)^2 g_e/\varepsilon)$ | $L^\infty$ |
| Fractional-query (BCCKS) | $\widetilde{O}(d^2 \|H\|_{\max,\infty} t\, n)$ | N/A | $L^\infty$ (gates) |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series (KSB/Low-Wiebe)]] | $\widetilde{O}(d \|H\|_{\max,\infty} t\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,\infty} t\, L^2 g_c)$ | $L^\infty$ |
| **Continuous qDRIFT (this paper)** | $\widetilde{O}((d^2 \|H\|_{\max,1})^2 n/\varepsilon)$ | $O(\|\alpha\|_{1,1}^2 g_e/\varepsilon)$ | **$L^1$** |
| **Rescaled Dyson (this paper)** | $\widetilde{O}(d\,\|H\|_{\max,1}\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,1} L^2 g_c)$ | **$L^1$** |

The improvement is the replacement $\|H\|_{\max,\infty} \cdot t \to \|H\|_{\max,1}$ everywhere. Since $\|H\|_{\max,1} \le t \cdot \|H\|_{\max,\infty}$ always, and the gap can be enormous when $H$ varies over time, this is a strict improvement.

## Application: chemical scattering

Section 5 identifies **semi-classical molecular scattering** as a natural use case. In a reactive collision, nuclei approach along a classical trajectory; the electronic Hamiltonian's norm peaks sharply during the brief near-collision phase and is small when nuclei are far apart. This creates exactly the scenario where $\|H\|_{\max,1} \ll t \cdot \|H\|_{\max,\infty}$.

The paper gives a schematic example of $H_2 + H$ scattering, where the Coulomb interactions vary as $1/R(t)$ along a classical nuclear trajectory. The L1-norm improvement captures that you only pay for the interaction when the atoms are actually close.

## Limits / caveats

- The rescaled algorithm requires oracle access to the cumulative norm function $f(t)$ and its inverse $f^{-1}(s)$. In practice, any efficiently computable upper bound $\Lambda(\tau) \ge \|H(\tau)\|_{\max}$ works, at the cost of a larger total simulation time $S = \int_0^t \Lambda(\tau) d\tau$.
- Continuous qDRIFT produces a **mixed-unitary channel**, not a deterministic unitary. Fine for simulation (diamond-norm error) but can't be used inside coherent algorithms.
- The quadratic-vs-linear gap between qDRIFT and rescaled Dyson can be large when $\|H\|_{\max,1}/\varepsilon$ is big — which is the typical regime for precision simulation.
- If $\|H(\tau)\|$ is approximately constant in time, the improvement over prior bounds is negligible (the $L^1$ and $L^\infty$ norms coincide up to a constant).
- The rescaled Dyson approach inherits the limitations of the underlying [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson-series algorithm]]: it requires continuously differentiable $H(\tau)$ with $H(\tau) \ne 0$ everywhere. Pathological cases (discontinuous norm, zero-crossings) need regularization.

## Reusable ideas

1. [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]] — sample simulation time proportional to instantaneous norm to avoid worst-case blowup
2. [[Integrated-Norm Equal-Segment Partitioning]] — split the time interval into equal-integrated-norm segments rather than uniform time slices
3. [[Time-Reparameterization for Norm-Flattened Dyson Simulation]] — reparameterize the Schrödinger equation via cumulative norm to flatten the Hamiltonian's norm profile, converting any simulation algorithm to $L^1$-norm scaling

## References within this paper

- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer & Berry (2019)]] — Dyson-series simulation of time-dependent Hamiltonians; this paper's rescaled algorithm builds directly on it
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — discrete qDRIFT; continuous qDRIFT generalizes this to time-dependent Hamiltonians
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2019)]] — interaction-picture Dyson; claimed $L^1$ scaling via segmentation but without formal proof
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin, Qarry, Somma & Verstraete (2011)]] — Hamiltonian averaging / Monte Carlo; continuous qDRIFT is compared to this approach in Appendix A
- Berry, Childs, Cleve, Kothari & Somma (2015) — fractional-query simulation; Theorem 4 shows its query complexity already has $L^1$ scaling
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP optimal scaling (comparison point)
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — commutator-based [[Product Formula Error Bounds|Trotter error theory]]; related analysis techniques for [[Product Formulas]]s
- Wiebe, Berry, Høyer & Sanders (2010) — higher-order [[Product Formulas]]s for time-dependent simulation; requires smoothness

---

## Cross-links

### Paper notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]

### Trick cards
- [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]]
- [[Integrated-Norm Equal-Segment Partitioning]]
- [[Time-Reparameterization for Norm-Flattened Dyson Simulation]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[Linear Combination of Unitaries (LCU)]]

### Comparison tables
- [[Hamiltonian Simulation — Comparison Tables]]
