> **Source:** Nathan Wiebe, Christopher Granade, Christopher Ferrie, D. G. Cory, *Hamiltonian Learning and Certification Using Quantum Resources*, arXiv:1309.0876, Phys. Rev. Lett. **112**, 190501 (2014)
> **Links:** [arXiv](https://arxiv.org/abs/1309.0876) · [PRL](https://doi.org/10.1103/PhysRevLett.112.190501)
> **Tags:** #hamiltonian-learning #quantum-simulation #Bayesian-inference #SMC #certification #IQLE #PGH

---

## The computational problem

**Hamiltonian learning:** Given an unknown, untrusted quantum device implementing some time-independent Hamiltonian $H_{\mathrm{true}} = H(\mathbf{x})$ drawn from a known parametric family $\{H(\mathbf{x}) : \mathbf{x} \in \mathbb{R}^d\}$, efficiently estimate $\mathbf{x}$ to precision $\delta$ in quadratic loss

$$L(\hat{\mathbf{x}}, \mathbf{x}) = \|\hat{\mathbf{x}} - \mathbf{x}\|^2$$

using adaptive experiments on the untrusted device combined with a *trusted* quantum simulator.

**Resources counted:**
- $N_{\mathrm{steps}}(\delta)$: number of experiments on the untrusted device
- $\mathrm{Cost}(\mathrm{update}; \varepsilon)$: trusted-simulator calls per Bayesian update (to estimate likelihoods to precision $\varepsilon$)
- Total: $N_{\mathrm{steps}}(\delta) \times \mathrm{Cost}(\mathrm{update}; \varepsilon)$

**Certification:** The same algorithm produces a credible region (posterior ellipse) that certifies $H_{\mathrm{true}}$ lies within it with probability $\alpha$, giving a practical tool for validating analog quantum simulators.

---

## What the paper does

Proposes an end-to-end algorithm for learning an unknown Hamiltonian that replaces intractable classical likelihood evaluation with quantum likelihood evaluation on a trusted simulator. The central insight: you don't need to solve an exponentially hard classical problem at each step — you just need to *simulate* the device with a guessed Hamiltonian and compare outcomes. Three experiment types are introduced (CLE, QLE, IQLE), with the Interactive Quantum Likelihood Evaluation (IQLE) + Particle Guess Heuristic (PGH) combination being the practical recommendation.

The efficiency argument rests on IQLE's Loschmidt-echo structure keeping likelihoods $O(1)$ (instead of exponentially small), combined with the exponential convergence of Sequential Monte Carlo (SMC) Bayesian updates. Numerics on frustrated Ising models up to 12 qubits ($d = O(n^2)$ parameters) confirm $N_{\mathrm{steps}}(\delta) \approx O(d \log(1/\delta))$.

This is the foundational Hamiltonian learning paper. The later [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes|bootstrapping paper]] extends it to compressed sensing; the [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes|Granade et al. (2012) paper]] is the direct precursor.

---

## The algorithm

### Experiment types

**CLE (Classical Likelihood Evaluation):** Compute $\Pr(D | \mathbf{x}_j) = |\langle D | e^{-iH(\mathbf{x}_j)t} | \psi \rangle|^2$ classically. Exact but intractable for large quantum systems.

**QLE (Quantum Likelihood Evaluation):** Use the trusted quantum simulator to prepare $|\psi\rangle$, evolve under $H(\mathbf{x}_j)$ for time $t$, measure outcome $D$. Repeat to estimate $\Pr(D | \mathbf{x}_j)$ by frequency. Efficient — but likelihoods can become exponentially small at long $t$ (Loschmidt echo problem).

**IQLE (Interactive Quantum Likelihood Evaluation):** Combine the untrusted device and trusted simulator in a Loschmidt-echo circuit. Swap the state $|\psi\rangle$ from the untrusted device into the trusted simulator, then apply the sequence

$$e^{+iH(\mathbf{x}^-)t} \cdot e^{-iH(\mathbf{x}_j)t}$$

so the measured likelihood becomes

$$\Pr(D | \mathbf{x}_j; \mathbf{x}^-, t) = \left|\langle D | e^{+iH(\mathbf{x}^-)t} e^{-iH(\mathbf{x}_j)t} | \psi \rangle\right|^2.$$

When $\mathbf{x}^- \approx \mathbf{x}_j$, the two evolutions nearly cancel and the amplitude is $O(1)$, avoiding exponentially small likelihoods.

### Particle Guess Heuristic (PGH)

The critical experiment-design rule:

1. Sample two particles $\mathbf{x}^-$, $\mathbf{x}^{-\prime}$ from the current posterior (weighted by $w_j$).
2. Set the inversion Hamiltonian $H^- = H(\mathbf{x}^-)$.
3. Set the evolution time $t = 1 / \|\mathbf{x}^{-\prime} - \mathbf{x}^-\|_2$.

As the posterior concentrates, $\|\mathbf{x}^{-\prime} - \mathbf{x}^-\|$ shrinks and $t$ grows adaptively. PGH ensures that at each step, the experiment has sensitivity tuned to the current uncertainty — exactly the idea behind optimal experiment design for phase estimation.

### Sequential Monte Carlo (SMC) Bayesian update

Represent $\Pr(\mathbf{x})$ as a particle approximation $\sum_j w_j \delta(\mathbf{x} - \mathbf{x}_j)$ with $N_p$ particles.

On observing outcome $D$:
$$w_j \leftarrow \Pr(D | \mathbf{x}_j)\, w_j, \quad \text{then normalise.}$$

When the effective sample size $N_{\mathrm{eff}} = 1/\sum_j w_j^2$ falls below $N_p/2$, apply Liu–West resampling:
- Resample particles according to weights.
- Jitter each particle: $\mathbf{x}_j \leftarrow a \mathbf{x}_j + (1-a)\bar{\mathbf{x}} + \boldsymbol{\eta}$, where $\bar{\mathbf{x}}$ is the posterior mean, $\boldsymbol{\eta} \sim \mathcal{N}(0, (1-a^2)\hat{\Sigma})$, and $a \approx 0.9$.
- Reset weights to $1/N_p$.

The posterior mean estimate at convergence is $\hat{H} = \sum_j w_j H(\mathbf{x}_j)$; the posterior covariance gives a certified credible region.

### Full algorithm

```
Initialise: prior Pr(x), N_p particles {x_j}, uniform weights w_j = 1/N_p

Repeat until Tr(Cov[x]) ≤ δ:
  1. PGH: sample x⁻, x⁻' from posterior; set t = 1/‖x⁻' − x⁻‖
  2. Run IQLE experiment on untrusted device: observe D
  3. For each particle x_j:
       estimate Pr(D | x_j; x⁻, t) via trusted simulator
  4. Update weights: w_j ← Pr(D | x_j) · w_j, normalise
  5. If N_eff < N_p/2: Liu–West resample

Return: posterior mean μ_H = Σ_j w_j H(x_j) and credible ellipse
```

---

## Key results

**Loschmidt-echo lower bound (Eq. 1):** For IQLE with inversion parameter $\mathbf{x}^-$,

$$\left|\langle \psi | e^{+iH(\mathbf{x}^-)t} e^{-iH(\mathbf{x})t} | \psi \rangle\right| \geq 1 - 2\|H(\mathbf{x}) - H(\mathbf{x}^-)\|_2\, t.$$

With PGH setting $t \approx 1/\|\mathbf{x} - \mathbf{x}^-\|$, the overlap stays $\Theta(1)$, keeping likelihoods away from exponentially small values.

**Update cost bound (Appendix D, Eq. D13–D14):** To estimate each particle's likelihood to within error $\varepsilon$ (in 1-norm of posterior update), the trusted simulator needs per-particle sample count

$$N_{\mathrm{samp}} \in \Omega\!\left(\frac{\max_k \Pr(D|\mathbf{x}_k)(1-\Pr(D|\mathbf{x}_k))}{\varepsilon^2 \left(\sum_k \Pr(D|\mathbf{x}_k)\Pr(\mathbf{x}_k)\right)^2}\right)$$

giving total simulator calls per update $\Theta(N_p \cdot N_{\mathrm{samp}})$.

**Empirical convergence (numerics):** Quadratic loss decays exponentially in $N_{\mathrm{steps}}$:

$$L \approx A\, e^{-\gamma N_{\mathrm{steps}}}$$

with $\gamma \sim O(1/d)$, implying $N_{\mathrm{steps}}(\delta) = O(d \log(1/\delta))$. This is consistent with the information-theoretic lower bound of $\Omega(d \log(1/\delta))$ (one bit per two-outcome measurement).

**Comparison table:**

| Method | Likelihood evaluation | Advantage | Limitation |
|---|---|---|---|
| CLE | Classical simulation | Exact | Exponentially expensive for large $n$ |
| QLE | Trusted quantum simulator | Efficient if likelihoods $O(1)$ | Long-$t$ overlaps exponentially small |
| IQLE + PGH | Trusted simulator + Loschmidt echo | Likelihoods $\Theta(1)$; adaptive $t$ | Requires swap / inversion capability |

---

## Caveats

1. **Trusted simulator required.** The quantum advantage relative to CLE requires a trusted quantum device (quantum computer or smaller trusted simulator). Not a near-term algorithm — needs fault-tolerant or very clean hardware.

2. **Known model family.** The algorithm learns parameters $\mathbf{x}$; it assumes the functional form $H(\mathbf{x})$ is known. It cannot discover Hamiltonian structure from scratch. For completely unknown Hamiltonians, you need compressed sensing (see [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes|bootstrapping paper]]).

3. **Dimension scaling.** $N_{\mathrm{steps}} = O(d \log 1/\delta)$ is efficient only when $d = O(\mathrm{poly}(n))$. For a fully connected $n$-qubit Ising model, $d = n(n-1)/2 = O(n^2)$; this is fine. For a completely general $n$-qubit Hamiltonian, $d = O(4^n)$ — not efficient.

4. **IQLE circuit.** The Loschmidt-echo construction requires swapping state from the untrusted device into the trusted simulator. This is a non-trivial quantum operation; not all experimental platforms support it.

5. **SMC particle count.** The number of particles $N_p$ needed for accurate posteriors grows with $d$. The authors argue (via cited SMC theory) this scales sub-exponentially in $d$, but in practice the particle count can be large.

6. **Multimodal posteriors.** PGH assumes the posterior is roughly unimodal. Multiple well-separated modes can cause experiment selection to fail and inference to get stuck.

---

## Reusable ideas

1. **IQLE Loschmidt-echo trick.** Wrap quantum likelihood evaluation in a Loschmidt-echo circuit to keep likelihoods $\Theta(1)$ throughout learning. The inversion Hamiltonian is chosen from the current prior — so the echo adapts as beliefs update. See [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]].

2. **Particle Guess Heuristic (PGH).** Set the experiment time $t = 1/\|\mathbf{x}^{-\prime} - \mathbf{x}^-\|$ by sampling two particles from the posterior. This adaptively tunes sensitivity to current uncertainty without solving an expensive experiment-design optimisation. See [[Particle Guess Heuristic for Adaptive Quantum Metrology]].

3. **Sequential Monte Carlo for Hamiltonian Bayesian inference.** SMC with Liu–West resampling gives a tractable representation of the posterior over Hamiltonian parameters that updates in $O(N_p)$ time per experiment and converges exponentially in the number of experiments. See [[Sequential Monte Carlo for Quantum Parameter Estimation]].

---

## References within this paper

- Granade, Ferrie, Wiebe, Cory (2012), "Robust Online Hamiltonian Learning", New J. Phys. — direct precursor; introduces the SMC + Bayesian framework for Hamiltonian learning without IQLE
- da Silva, Landon-Cardinal, Poulin (2011) — classical characterisation of local Hamiltonians; the approach being surpassed
- Liu and West (2001) — the Liu–West resampler used in SMC
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — universal quantum simulators; motivation for certification
- Hentschel & Sanders (2010, 2011) — machine learning for quantum metrology; related adaptive experiment design
- Ferrie and Granade, "How to best sample a periodic probability distribution" (2012) — single-parameter experiment design analysis

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]] — same year, same first author; data fitting paper uses HHL for linear algebra, this paper uses Bayesian inference for system identification — two complementary directions in early quantum ML
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — contemporary work; Wiebe simultaneously developing simulation primitives that underlie IQLE
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — certification of quantum simulators is the motivating application
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — later Wiebe simulation work; more efficient simulation of the IQLE evolutions
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — modern simulation framework that would accelerate the trusted-simulator step
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — related "learn what you need without full tomography" philosophy
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — PAC learning of quantum states; complementary approach to learning quantum system behaviour

### Trick cards
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
- [[Noise-Robust Bayesian Hamiltonian Learning]] — from sequel paper on noisy resources
- [[Superoperator Noise Incorporation for Robust QHL]] — from sequel paper on noisy resources
- [[Bayesian Model Selection via SMC Marginal Likelihoods]] — from sequel paper on noisy resources
- [[Rejection Filtering for Lightweight Bayesian Updates]] — RFPE distills this paper's Bayesian/PGH framework to single-parameter PE with a Gaussian posterior
- [[PGH Adaptation for Single-Parameter Phase Estimation]] — closed-form PGH for phase estimation, derived from the multi-parameter PGH introduced here

### Subsequent work
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]] — applies the QHL Bayesian/PGH framework to single-parameter phase estimation; replaces full SMC with Gaussian + rejection sampling (RFPE)
