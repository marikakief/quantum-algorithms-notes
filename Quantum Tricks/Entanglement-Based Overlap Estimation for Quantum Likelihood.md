# Entanglement-Based Overlap Estimation for Quantum Likelihood

> **Source:** Wang, Paesani, Santagati et al., arXiv:1703.05402 (2017); technique from Cai et al., PRL 114, 110504 (2015)
> **Tags:** #trick #hamiltonian-learning #overlap-estimation #photonic #entanglement #IQLE

## What it does
Estimates $|\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle|^2$ using an entangled ancilla qubit and single-qubit measurements, without requiring time-reversal evolution.

## The trick

Prepare the entangled state
$$\frac{1}{\sqrt{2}}\bigl(|0_s\rangle\hat{U}|\psi_i\rangle + |1_s\rangle\hat{V}|\psi_i\rangle\bigr)$$
where $s$ is a signal (control) qubit and $i$ is the idler (target) qubit. Measuring the signal qubit extracts the overlap:

- **$\hat{\sigma}_x$ basis:** $\mathrm{Re}(\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle) = 2p_+ - 1$
- **$\hat{\sigma}_y$ basis:** $\mathrm{Im}(\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle) = 2p_{+i} - 1$

Combining both gives the modulus squared:
$$|\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle|^2 = (2p_+ - 1)^2 + (2p_{+i} - 1)^2.$$

**Application to QLE:** Set $\hat{U} = \hat{\mathbb{1}}$, $\hat{V} = e^{-iH(\mathbf{x})t}$ to get $|\langle\psi|e^{-iH(\mathbf{x})t}|\psi\rangle|^2$.

**Application to IQLE:** Set $\hat{U} = e^{-iH(\mathbf{x}^-)t}$, $\hat{V} = e^{-iH(\mathbf{x})t}$ to get the [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|Loschmidt-echo likelihood]] $|\langle\psi|e^{iH(\mathbf{x}^-)t}e^{-iH(\mathbf{x})t}|\psi\rangle|^2$ — but with both evolutions running forward in time. The entanglement handles the "inversion" that would otherwise require a backward Hamiltonian evolution.

## When to reach for it

- Implementing [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] on platforms where time reversal is difficult or impossible (e.g., analogue quantum simulators).
- Any quantum likelihood estimation task where you need $|\langle\psi|U^\dagger V|\psi\rangle|^2$ and can prepare entangled control-target states.
- Photonic platforms where entangled photon pairs and single-qubit measurements are natural operations.

## Complexity

- Two measurement settings ($\hat{\sigma}_x$ and $\hat{\sigma}_y$ on the control qubit) per likelihood evaluation.
- $O(1/\varepsilon^2)$ shots per setting for $\varepsilon$-accurate probability estimation (standard binomial sampling).
- One entangled pair consumed per shot.
- Total cost per likelihood: $O(1/\varepsilon^2)$ entangled pairs and forward-time evolutions.

## Caveat

- Requires an entangled ancilla qubit and post-selected coincidence detection (in the photonic implementation). This limits count rates and introduces post-selection overhead.
- The $\hat{U}$ and $\hat{V}$ operations must be coherently controlled by the ancilla qubit. For multi-qubit target systems, this means controlled multi-qubit unitaries — a non-trivial circuit overhead.
- Whether this forward-only variant inherits the full Loschmidt-echo overlap bound from [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|standard IQLE]] has not been rigorously analysed.

## Related notes
- [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes]] — The experiment where this trick was demonstrated
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]] — The standard IQLE construction using time reversal
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — Theory paper defining QLE/IQLE
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]] — Used with this trick for adaptive experiment design
