> **Source:** Jianwei Wang, Stefano Paesani, Raffaele Santagati, Sebastian Knauer, Antonio A. Gentile, Nathan Wiebe, Maurangelo Petruzzella, Jeremy L. O'Brien, John G. Rarity, Anthony Laing, Mark G. Thompson, *Experimental Quantum Hamiltonian Learning*, arXiv:1703.05402, Nature Physics **13**, 551–555 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1703.05402) · [Nature Physics](https://doi.org/10.1038/nphys4074)
> **Tags:** #hamiltonian-learning #experiment #photonic #NV-centre #Bayesian-inference #SMC #QLE #IQLE #certification #silicon-photonics

---

## The computational problem

Given an unknown quantum system whose dynamics are governed by a time-independent Hamiltonian $H_0 = H(\mathbf{x}_0)$ from a known parametric family, learn the parameters $\mathbf{x}_0$ by combining adaptive experiments on the system with a programmable quantum simulator. This is the experimental realisation of the [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory (2014)]] protocol.

The specific system under test: the electron spin of a nitrogen-vacancy (NV$^-$) centre in diamond, with Hamiltonian $\hat{H}(f) = \hat{\sigma}_x f / 2$ describing Rabi oscillations between the $m_s = 0$ and $m_s = -1$ ground-state spin levels.

---

## What the paper does

First experimental demonstration of quantum Hamiltonian learning (QHL). A silicon-photonics chip acts as a trusted quantum simulator to learn the Rabi frequency of an NV$^-$ centre's electron spin — two physically distinct quantum systems interfaced through a classical computer. Three things stand out:

1. **QLE works.** The photonic simulator learns the NV$^-$ Rabi frequency $f_{\mathrm{QLE}} = (6.93 \pm 0.09)$ MHz in 50 steps with 20 particles, consistent with the reference value $f_0 = 6.90$ MHz from a full Rabi fit. Quadratic loss reaches $\sim 10^{-5}$.

2. **Model deficiency detection.** After $\sim 35$ steps the variance saturates at $\sigma^2 \approx 4.2 \times 10^{-5}$, signalling that the simple $\hat{H}(f) = \hat{\sigma}_x f/2$ model (Model I) has hit its limits. Introducing chirping — $\hat{H}'(f, \alpha; t) = \hat{\sigma}_x(f + \alpha t)/2$ (Model II) — lets learning continue, reaching covariance norm $\|\Sigma\|_2 = 7.5 \times 10^{-6}$. The Bayes factor $K = 560$ strongly favours Model II. This validates the [[Bayesian Model Selection via SMC Marginal Likelihoods|Bayesian model selection]] capability predicted by [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe et al. (2014, PRA)]].

3. **IQLE self-verification.** The [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] protocol is implemented entirely on the photonic chip, with calibrated gates playing the trusted simulator and an uncalibrated $\hat{\sigma}_x$-rotation as the "untrusted system." Convergence to the correct frequency $f_{\mathrm{IQLE}} = (6.92 \pm 0.08)$ MHz with quadratic loss $\sim 10^{-7}$ — two orders of magnitude better than QLE — demonstrates the protocol's value for quantum device self-verification.

My assessment: this is a clean proof-of-concept that closes the theory→experiment loop for [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe et al. (2014)]]. The system learned is simple (one or two parameters, single qubit), but what matters is that the full Bayesian inference pipeline works end-to-end on real hardware, including the model selection and self-verification aspects. The model deficiency detection is the most interesting experimental finding — it shows QHL doesn't just fit parameters but tells you when your model is wrong.

---

## The experiment

### Hardware

**Trusted simulator:** A silicon-on-insulator (SOI) quantum photonic chip fabricated via 248 nm UV photolithography. Path-encoded qubits in 450 nm × 220 nm silicon waveguides. Photon pairs generated via spontaneous four-wave mixing in 1.2 cm spiral sources (near 1550 nm), split by multi-mode interferometer (MMI) beam splitters to produce post-selected maximally entangled states $(|0_s\rangle|0_i\rangle + |1_s\rangle|1_i\rangle)/\sqrt{2}$ (signal + idler photons). Thermo-optical phase shifters (titanium, 12-bit DAC resolution) control single- and two-qubit operations. Detection via off-chip fibre-coupled superconducting nanowire single-photon detectors (SNSPDs).

**Untrusted system (QLE):** NV$^-$ centre in CVD-grown electronic-grade bulk diamond (1 ppb nitrogen). Room temperature, 5 mT external magnetic field aligned to NV axis (splitting $m_s = \pm 1$ degeneracy). Transition frequency 2.742 GHz. Optical initialisation into $m_s = 0$ via 532 nm laser, microwave manipulation, photoluminescence readout via avalanche photodiode. Each measurement outcome from $3 \times 10^6$ repetitions.

**Untrusted system (IQLE):** An uncalibrated single-qubit gate on the same photonic chip. The chip plays both roles — trusted and untrusted — enabling self-verification.

### Likelihood estimation on the photonic chip

The chip implements an entanglement-based overlap estimation scheme. Given the entangled state
$$\frac{1}{\sqrt{2}}(|0_s\rangle\hat{U}|\psi_i\rangle + |1_s\rangle\hat{V}|\psi_i\rangle),$$
measuring the signal qubit in the $\hat{\sigma}_x$ basis gives
$$\mathrm{Re}(\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle) = 2p_+ - 1,$$
and in the $\hat{\sigma}_y$ basis gives
$$\mathrm{Im}(\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle) = 2p_{+i} - 1.$$

For **QLE**: set $\hat{U} = \hat{\mathbb{1}}$, $\hat{V} = e^{-i\hat{H}(\mathbf{x})t}$, giving
$$L_{\mathrm{QLE}} = |\langle\psi|e^{-i\hat{H}(\mathbf{x})t}|\psi\rangle|^2 = (2p_+ - 1)^2 + (2p_{+i} - 1)^2.$$

For **IQLE**: set $\hat{U} = e^{-i\hat{H}(\mathbf{x}^-)t}$, $\hat{V} = e^{-i\hat{H}(\mathbf{x})t}$, giving
$$L_{\mathrm{IQLE}} = |\langle\psi|e^{i\hat{H}(\mathbf{x}^-)t}e^{-i\hat{H}(\mathbf{x})t}|\psi\rangle|^2.$$

All evolutions are forward in time (unlike the original [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] proposal which requires time reversal). The entanglement-based approach replaces the backward evolution with a controlled unitary on the ancilla, making it more amenable to analogue quantum simulators. The cost is that it requires an entangled ancilla.

### Bayesian inference

[[Sequential Monte Carlo for Quantum Parameter Estimation|Sequential Monte Carlo]] with 20 particles, Liu–West resampling. Prior: uniform over $\omega = f/\Delta f \in [0,1]$ where $\Delta f = 100/2\pi$ MHz. Evolution times chosen adaptively (though the paper uses a simpler time schedule than the full [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]] for the QLE demonstration; full PGH is used for IQLE).

---

## Key results

| Protocol | System learned | Learned value | Reference value | Quadratic loss | Steps |
|---|---|---|---|---|---|
| QLE (Model I) | NV$^-$ Rabi frequency | $(6.93 \pm 0.09)$ MHz | 6.90 MHz | $\sim 10^{-5}$ | 50 |
| QLE (Model II, chirped) | NV$^-$ Rabi + chirp | $f = (7.00 \pm 0.04)$ MHz, $\alpha = (-0.26 \pm 0.04)$ MHz$^2$ | $f_0 = 6.94$ MHz, $\alpha_0 = -0.28$ MHz$^2$ | $\|\Sigma\|_2 = 7.5 \times 10^{-6}$ | 50 |
| IQLE | Photonic $\hat{\sigma}_x$-rotation | $(6.92 \pm 0.08)$ MHz | 6.90 MHz | $\sim 10^{-7}$ | 50 |

**Convergence behaviour:**
- QLE shows exponential decay of quadratic loss for the first ~35 steps, consistent with theoretical predictions (500-run simulation averaged).
- Saturation at step ~35 for Model I is clean and unambiguous — the variance stops decreasing. This is a textbook example of model deficiency detection within a Bayesian framework.
- IQLE achieves two orders of magnitude better final loss ($10^{-7}$ vs $10^{-5}$), validating the theoretical advantage of the Loschmidt-echo approach.
- Bayes factor $K = 560$ between Models I and II — strong evidence for chirping in the microwave drive.

**Comparison with theory:** The experimental convergence rates sit within the 67.5% confidence interval of the 500-run theoretical simulation for both QLE and IQLE. The theory predicted exponential convergence in experiment number; the experiment confirms this.

---

## Comparison with prior work

| Method | What it does | Limitation this paper addresses |
|---|---|---|
| [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes\|Wiebe et al. (2014, PRL)]] | Theory: QLE/IQLE + PGH + SMC for Hamiltonian learning | No experimental demonstration |
| [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes\|Wiebe et al. (2014, PRA)]] | Theory: robustness to noise, model selection | Not demonstrated on real hardware |
| [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes\|Granade et al. (2012)]] | SMC Bayesian framework, classical likelihood | No quantum simulator in the loop |
| da Silva, Landon-Cardinal, Poulin (2011) | Classical Hamiltonian characterisation | Exponentially expensive for large systems |
| Standard Rabi fitting | Fit oscillation data | Doesn't use Bayesian model selection; can't detect model deficiencies as cleanly |

This paper doesn't improve any algorithmic bounds — it validates them. The contribution is experimental: demonstrating that the full QHL pipeline (adaptive experiments → quantum likelihood estimation → Bayesian update → model selection) works on real hardware with real noise.

---

## Limits / caveats

1. **Single-qubit system.** The NV$^-$ Hamiltonian has 1–2 parameters. The theoretical advantage of QHL over classical likelihood evaluation is for multi-qubit systems where classical simulation is intractable. For a single qubit, classical simulation is trivial — the quantum simulator doesn't save anything computationally. This is a proof of concept, not a demonstration of quantum advantage.

2. **No quantum channel between systems.** The NV$^-$ and photonic chip are interfaced classically (measurement outcomes sent to classical computer). The IQLE demonstration uses the photonic chip for both roles. True cross-platform IQLE — where the untrusted system's quantum state is swapped into the trusted simulator via a quantum channel — was not demonstrated. This is the harder version of the protocol.

3. **Photonic chip limitations.** Post-selected entanglement (not deterministic). Low count rates requiring long integration times. Thermo-optical phase shifters are slow and have finite resolution (12 bits). These don't affect the algorithm's correctness but limit scalability.

4. **SMC with 20 particles.** Very low particle count by SMC standards. Works for 1–2 parameters but would need to grow for higher-dimensional Hamiltonian families. The paper doesn't address how particle count scales with dimension in practice.

5. **NV$^-$ measurement overhead.** Each measurement outcome requires $3 \times 10^6$ repetitions of the Rabi sequence. The total measurement budget exceeds what a simple Rabi fit requires (the paper notes $\sim 200$ measurements for the fit). The advantage is supposed to emerge for larger systems where the number of QHL experiments scales as $O(d \log(1/\delta))$.

6. **Forward-only IQLE.** The entanglement-based scheme avoids time reversal but requires an entangled ancilla and post-selection. Whether this variant inherits all the theoretical guarantees of the standard [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] construction (particularly the Loschmidt-echo lower bound on overlap) deserves more analysis.

---

## Reusable ideas

1. [[Entanglement-Based Overlap Estimation for Quantum Likelihood]] — Use an entangled signal-idler pair to estimate $|\langle\psi|\hat{U}^\dagger\hat{V}|\psi\rangle|^2$ by measuring the signal qubit in the $\hat{\sigma}_x$ and $\hat{\sigma}_y$ bases. Avoids the need for time-reversal evolution in IQLE by replacing it with a controlled unitary on the ancilla qubit.

2. [[Variance Saturation as Model Deficiency Diagnostic]] — In Bayesian parameter estimation, when the posterior variance saturates rather than continuing to decrease exponentially, this signals that the model has insufficient parameters to describe the true dynamics. Introduce additional parameters (here: chirping) and compare models via the Bayes factor from [[Bayesian Model Selection via SMC Marginal Likelihoods|SMC marginal likelihoods]].

---

## References within this paper

Key cited works with vault links:

- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe, Granade, Ferrie, Cory (2014, PRL)]] — The foundational QHL theory paper that this experiment validates. Introduces QLE, IQLE, PGH, and the $O(d \log 1/\delta)$ experiment count.
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe, Granade, Ferrie, Cory (2014, PRA)]] — Noise robustness and model selection theory. The chirping detection and Bayes factor comparison validate predictions from this paper.
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — Motivation: certification of quantum simulators.
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]] — Prior photonic quantum simulation experiment.
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al. (2010)]] — Earlier photonic quantum chemistry experiment.
- Cai et al. (2015), "Entanglement-based machine learning on a quantum computer" — The entanglement-based overlap estimation technique used here.
- da Silva, Landon-Cardinal, Poulin (2011) — Classical Hamiltonian characterisation without quantum resources.

---

## Cross-links

### Paper notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — The theory paper this experiment validates
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — Noise robustness theory; the model selection capability validated here
- [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes]] — The SMC framework underlying both theory and experiment
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]] — Later distillation of the Bayesian/PGH framework to single-parameter phase estimation
- [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes]] — Extension to compressed sensing for unknown Hamiltonian structure

### Trick cards
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]] — The core technique; this paper demonstrates the entanglement-based variant
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]] — Used for IQLE experiment design
- [[Sequential Monte Carlo for Quantum Parameter Estimation]] — The Bayesian engine driving the inference
- [[Bayesian Model Selection via SMC Marginal Likelihoods]] — Model comparison (Model I vs Model II) demonstrated here
- [[Entanglement-Based Overlap Estimation for Quantum Likelihood]] — New trick card: the forward-only IQLE variant
- [[Variance Saturation as Model Deficiency Diagnostic]] — New trick card: using posterior saturation to diagnose model failure
