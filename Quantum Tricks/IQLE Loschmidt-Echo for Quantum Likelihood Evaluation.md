# IQLE Loschmidt-Echo for Quantum Likelihood Evaluation

> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1309.0876 (2014)
> **Tags:** #trick #hamiltonian-learning #IQLE #Loschmidt-echo #quantum-simulation #certification #likelihood

## What it does
Keeps quantum likelihoods $\Theta(1)$ throughout Hamiltonian learning by wrapping each likelihood evaluation in a Loschmidt echo — running both the *guessed* Hamiltonian and the *inverse* of a reference Hamiltonian — so the two evolutions partially cancel and the measured overlap stays well away from zero.

## The trick

**Problem with naive QLE:** When estimating $\Pr(D | \mathbf{x}_j) = |\langle D | e^{-iH(\mathbf{x}_j)t} | \psi \rangle|^2$ at large $t$ (needed for high precision), the overlap $\langle \psi | e^{-iH(\mathbf{x}_j)t} | \psi \rangle$ can be exponentially small in $n$ for generic quantum states, making probability estimation exponentially expensive.

**IQLE construction:**
1. Swap state $|\psi\rangle$ from the untrusted device into the trusted quantum simulator.
2. Apply the *inversion* evolution $e^{+iH(\mathbf{x}^-)t}$ (running the guessed reference Hamiltonian backward).
3. Apply the forward evolution $e^{-iH(\mathbf{x}_j)t}$ for the candidate particle.
4. Measure outcome $D$.

The measured likelihood is

$$\Pr(D | \mathbf{x}_j;\, \mathbf{x}^-, t) = \left|\langle D | e^{+iH(\mathbf{x}^-)t}\, e^{-iH(\mathbf{x}_j)t} | \psi \rangle\right|^2.$$

**Why it works — Loschmidt lower bound:**

$$\left|\langle \psi | e^{+iH(\mathbf{x}^-)t}\, e^{-iH(\mathbf{x})t} | \psi \rangle\right| \geq 1 - 2\|H(\mathbf{x}) - H(\mathbf{x}^-)\|_2\, t.$$

If $\mathbf{x}^- \approx \mathbf{x}$ (reference particle near the true parameter), the two unitaries nearly cancel and the amplitude is $O(1)$. When the inversion Hamiltonian is chosen from the posterior via [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]], $\|\mathbf{x}^- - \mathbf{x}\| = O(1/t)$ on average, keeping the overlap bounded away from zero throughout inference.

## When to reach for it

- Bayesian Hamiltonian learning with $n$-qubit systems where classical likelihood evaluation is intractable.
- Certifying an unknown analog quantum simulator using a trusted one.
- Any adaptive parameter estimation scenario where experiments at long time $t$ are needed but direct overlaps are too small.
- More broadly: when you need to estimate $|\langle \psi | U_\mathrm{true} U_\mathrm{guess}^\dagger | \psi \rangle|^2$ and the individual overlaps $|\langle \psi | U | \psi \rangle|^2$ are small.

## Complexity

- One IQLE experiment: one run of untrusted device + $N_\mathrm{samp}$ runs of trusted simulator.
- $N_\mathrm{samp} = O(1/\varepsilon^2)$ per particle (binomial sampling) when likelihoods are $\Theta(1)$.
- Without IQLE (plain QLE at long $t$): $N_\mathrm{samp}$ must grow exponentially in $n$ to sample rare events — removes quantum advantage.
- Total experiments: $O(d \log 1/\delta)$ to reach quadratic loss $\delta$ with $d$ parameters (empirically confirmed).

## Caveat

- Requires a *trusted* quantum simulator capable of implementing controlled inversions (backward evolution), plus a swap operation between the untrusted device and the trusted simulator. This is a hardware requirement, not a pure algorithmic one.
- The echo construction only helps when the posterior is unimodal near the true parameter (so that most reference particles $\mathbf{x}^-$ are nearby). Early in inference, the benefit is reduced; you may need a warm-start or coarse initialisation.
- The Loschmidt bound is tight only if $\mathbf{x}^-$ is close to the true $\mathbf{x}$. PGH delivers this once the posterior has concentrated; for very broad priors, initial experiments still have $O(1)$ likelihoods but carry less information per shot.

## Related notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — extends IQLE to noisy settings; depolarizing robustness and swap-error modeling
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
- [[Noise-Robust Bayesian Hamiltonian Learning]]
- [[Superoperator Noise Incorporation for Robust QHL]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — efficient simulation of the forward/backward evolutions needed here
