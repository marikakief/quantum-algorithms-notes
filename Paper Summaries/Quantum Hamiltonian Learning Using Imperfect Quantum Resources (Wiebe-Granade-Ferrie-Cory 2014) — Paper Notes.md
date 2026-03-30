> **Source:** Nathan Wiebe, Christopher Granade, Christopher Ferrie, David G. Cory, *Quantum Hamiltonian Learning Using Imperfect Quantum Resources*, arXiv:1311.5269, Phys. Rev. A **89**, 042314 (2014)
> **Links:** [arXiv](https://arxiv.org/abs/1311.5269) · [PRA](https://doi.org/10.1103/PhysRevA.89.042314)
> **Tags:** #hamiltonian-learning #quantum-simulation #Bayesian-inference #SMC #IQLE #noise-robustness #model-selection #depolarizing #swap-error

---

## The computational problem

Same setting as [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|the companion PRL paper]]: learn the parameters $\mathbf{x} \in \mathbb{R}^d$ of a known parametric Hamiltonian $H(\mathbf{x})$ describing an unknown device, using adaptive measurements and a trusted quantum simulator. The critical new question here: **does the algorithm still work when the quantum resources are imperfect?**

Specifically: what happens when
1. the IQLE experiments are subject to **depolarizing noise** (on the untrusted device or trusted simulator)?
2. the **swap gate** between untrusted device and trusted simulator is noisy (realistic multi-qubit swap fidelity < 1)?
3. the **model is wrong** — the true Hamiltonian $\tilde{H}$ is not in the family $\{H(\mathbf{x})\}$?
4. you want to do **model selection** between two competing Hamiltonian families?

---

## What the paper does

Establishes that [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] + [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]] + [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] learning is **robust to substantial noise**. The main findings:

- **Depolarizing noise** of visibility $\mathcal{N}$ slows learning by factor $(1 - \mathcal{N})$ but does not prevent convergence. Even 50% depolarization still works.
- **Noisy swap gates** (modeled via experimentally extracted superoperators) leave learning performance nearly intact for realistic devices (superconducting qubits, quantum dots).
- **Model mismatch** causes the posterior to saturate at a resolution floor set by $\|H_\mathrm{true} - H(\mathbf{x}^*)\|$, not to diverge or give wrong answers.
- **Bayesian model selection** (Bayes factors) is available essentially for free as a byproduct of [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] — the marginal likelihood normalisation constants suffice.

This is the companion journal paper to arXiv:1309.0876 (PRL). The PRL established the noiseless algorithm; this PRA paper makes the robustness case that gets it toward practical deployment.

---

## The algorithm

The core algorithm (Algorithm 1) is identical to [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|the PRL paper]]. I reproduce the key structure and focus on the extensions.

### Base algorithm (QHL)

Initialise $N_p$ particles $\{\mathbf{x}_j\}$ with weights $\{w_j = 1/N_p\}$ from prior.

For each experiment $i = 1, \ldots, N_\mathrm{exp}$:
1. **PGH experiment design:** draw $\mathbf{x}^-, \mathbf{x}^{-\prime}$ from posterior; set $t = 1/\|H(\mathbf{x}^{-\prime}) - H(\mathbf{x}^-)\|$.
2. **IQLE measurement:** evolve untrusted system, swap state to trusted simulator, apply inverse $e^{+iH(\mathbf{x}^-)t}$, measure outcome $D$.
3. **Likelihood update:** for each particle $j$, estimate $p_j = \Pr(D|\mathbf{x}_j; \mathbf{x}^-, t)$ via $N_\mathrm{samp}$ trusted-simulator runs.
4. **Weight update:** $w_j \leftarrow w_j p_j / Z$ where $Z = \sum_k w_k p_k$.
5. **Liu–West resample** if $N_\mathrm{eff} < N_p/2$.

Return posterior mean $\hat{\mathbf{x}} = \sum_j w_j \mathbf{x}_j$.

### Noisy IQLE model (depolarizing)

If the untrusted device introduces depolarizing noise with **visibility** $\mathcal{N}$ (fraction of coherent component preserved), the measured state before inversion is

$$\rho = (1 - \mathcal{N})\, |\psi(t)\rangle\langle\psi(t)| + \frac{\mathcal{N}}{2^n} I.$$

The IQLE outcome probability becomes (equation 10):

$$\Pr(D | \mathbf{x}; \mathcal{N}) = (1 - \mathcal{N})\, \Pr(D | \mathbf{x}; \mathcal{N}=0) + \frac{\mathcal{N}}{2^n}.$$

For the typical IQLE outcome $D = |0\rangle$ (Loschmidt echo), this gives (equation 11):

$$\Pr(0 | \mathbf{x}; \mathcal{N}) = (1 - \mathcal{N})\frac{1 + F(t,\mathbf{x},\mathbf{x}^-)}{2} + \frac{\mathcal{N}}{2^n}$$

where $F(t,\mathbf{x},\mathbf{x}^-) = |\langle\psi_0| e^{+iH(\mathbf{x}^-)t} e^{-iH(\mathbf{x})t} |\psi_0\rangle|^2$ is the noiseless echo fidelity.

The expected weight update magnitude (equations 12–13) is reduced approximately by factor $(1 - \mathcal{N})$:

$$\mathbb{E}[w_j \text{ update}] \approx (1 - \mathcal{N}) \times (\text{noiseless update})$$

so the learning rate $\gamma$ (in $\delta \sim e^{-\gamma N}$) scales as approximately $(1 - \mathcal{N})\gamma_0$. See [[Noise-Robust Bayesian Hamiltonian Learning]].

### Noisy swap gate modeling

Realistic swap gates are modeled using a **cumulant expansion / superoperator** approach (equations 14–17). The noisy swap is extracted experimentally (via quantum process tomography or GRAPE pulse simulation) and compressed to a superoperator $\Lambda_\mathrm{noise}$. This $\Lambda_\mathrm{noise}$ is then used directly inside Algorithm 1 — the algorithm updates its likelihood model with the noisy swap baked in. No special modification of the outer algorithm is required; only the inner simulator model changes.

The paper validates this with three physical platforms:
- **Quantum dots:** swap fidelity $F \approx 0.973$
- **Superconducting primitive:** $F \approx 0.906$–$0.996$
- **SC XY4 decoupling:** $F \approx 0.998$

In all cases, quadratic loss decays rapidly (reaching $\sim 10^{-6}$ in $N \approx 200$ experiments for a 2-qubit $J$-coupling). See [[Superoperator Noise Incorporation for Robust QHL]].

---

## Key results

**Depolarizing robustness (main theoretical result, equations 9–13):**

For depolarizing visibility $\mathcal{N}$, the per-experiment learning rate scales as

$$\gamma(\mathcal{N}) \approx (1 - \mathcal{N})\, \gamma_0.$$

Even $\mathcal{N} = 0.5$ (50% depolarization) leaves $\gamma = \gamma_0/2$ — learning is twice as slow, not broken.

**Experimental convergence (empirical, Figure 6):**

For random 4-qubit Ising Hamiltonians with depolarizing noise $\mathcal{N} \in \{0, 0.1, 0.3, 0.5\}$: quadratic loss decays exponentially in $N$ with rate proportional to $(1-\mathcal{N})$. Median $\gamma$ matches prediction.

**Model mismatch bound (equations 25–29):**

Let the true Hamiltonian be $\tilde{H}$ and the best-in-class model be $H^* = H(\mathbf{x}^*)$ with residual $\Delta H = \|\tilde{H} - H^*\|$. The likelihood error from mismatch satisfies (equation 26):

$$|\Pr(D | \tilde{H}; t) - \Pr(D | H^*; t)| \leq \|\tilde{H} - H^*\|^2 t^2.$$

With PGH setting $t \sim 1/\Delta H_\mathrm{posterior}$, this is bounded while $\Delta H \lesssim \Delta H_\mathrm{posterior}$. Once posterior uncertainty drops below the mismatch scale $\Delta H$, learning saturates at (equation 29):

$$\|\hat{H} - H^*\| \lesssim R\, \max_j \|H_j\|$$

where $R$ is the number of missing Hamiltonian terms. The algorithm converges to the closest model in the class, not a random wrong answer.

**Bayesian model selection (equations 32–37):**

The Bayes factor between model $\mathcal{M}_1$ and $\mathcal{M}_2$ given data $\mathbf{D}$ is

$$B_{12} = \frac{\Pr(\mathbf{D} | \mathcal{M}_1)}{\Pr(\mathbf{D} | \mathcal{M}_2)} = \frac{\Pr(\mathcal{M}_1 | \mathbf{D})}{\Pr(\mathcal{M}_2 | \mathbf{D})} \cdot \frac{\Pr(\mathcal{M}_2)}{\Pr(\mathcal{M}_1)}.$$

The marginal likelihood $\Pr(\mathbf{D} | \mathcal{M}) = \prod_i Z_i$ where $Z_i = \sum_k w_k p_k$ is the normalisation constant at step $i$. **This is already computed during SMC** — no extra work. Running two particle filters (one per model) and tracking their $Z_i$ products gives the Bayes factor at every step. See [[Bayesian Model Selection via SMC Marginal Likelihoods]].

Numerics (Figures 10–11): Bayes factors discriminate correct from reduced/overfitted models with overwhelming odds ($> 10^{120}:1$) within ~200 experiments.

---

## Comparison with prior work

| | [arXiv:1309.0876] PRL 2014 | This paper (PRA 2014) |
|---|---|---|
| Algorithm | QHL (IQLE + PGH + SMC) | Same |
| Noise | Assumed zero | Depolarizing + swap-gate errors |
| Model | Exact (known family) | Model mismatch treated |
| Selection | Not addressed | Bayes factors via SMC |
| Hardware | Theoretical | Validated on SC, quantum-dot superoperators |
| Main contribution | Algorithm + noiseless analysis | Robustness + practical applicability |

The paper explicitly positions itself as the "noise" sequel: "A major caveat of that work is the assumption that all elements of the protocol are noise free. Here we show that quantum Hamiltonian learning can tolerate substantial amounts of depolarizing noise."

---

## Limits / caveats

1. **Trusted simulator still required.** Robustness to noise on the *untrusted* side doesn't help if you have no reliable quantum simulator for likelihood evaluation. The algorithm degrades to classical (intractable) if the trusted simulator is absent.

2. **Depolarizing-specific analysis.** The noise model treated analytically is depolarizing. For correlated noise, non-Markovian errors, or coherent miscalibrations, the analysis must be redone. Numeric evidence (Figure 7) is more general (superoperator model), but formal guarantees are only for depolarizing.

3. **Saturation at mismatch scale.** Model mismatch produces a hard floor; if the missing terms matter physically, the floor may be unacceptably high. Compressed sensing (see [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes|bootstrapping paper]]) is the fix for sparse unknown structure.

4. **Liu–West resampling bias.** The jittering step in SMC introduces a small bias in the posterior. For very high precision requirements, this may matter. The parameter $a \approx 0.9$ is chosen empirically; no systematic analysis of the bias is given.

5. **Model selection is not model discovery.** Bayes factors compare within a menu of provided models. If the true Hamiltonian is not in any model class offered, model selection returns the least-wrong option, not the truth.

6. **Particle count not proven to scale well.** Sub-exponential $N_p$ scaling in $d$ is expected from SMC theory but not proved in this paper for the specific QHL setting.

---

## Reusable ideas

1. **Noise-robust Bayesian learning via linear likelihood rescaling.** Depolarizing noise rescales likelihoods by $(1-\mathcal{N})$ without changing their relative shape — so Bayesian inference is robust to this noise, just slower. See [[Noise-Robust Bayesian Hamiltonian Learning]].

2. **Superoperator incorporation for realistic noise.** Extract the noisy channel as a superoperator (from process tomography or GRAPE simulation), then plug it directly into the likelihood model. The outer algorithm is unchanged. See [[Superoperator Noise Incorporation for Robust QHL]].

3. **SMC marginal likelihoods for free model selection.** The normalisation constants $Z_i$ produced by SMC weight updates accumulate into the marginal likelihood. Running two particle filters in parallel gives Bayes factors at no algorithmic overhead beyond the second filter. See [[Bayesian Model Selection via SMC Marginal Likelihoods]].

4. **IQLE Loschmidt-Echo trick** — carried over from arXiv:1309.0876. See [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]].

5. **Particle Guess Heuristic (PGH)** — carried over. See [[Particle Guess Heuristic for Adaptive Quantum Metrology]].

6. **Sequential Monte Carlo (SMC)** — carried over. See [[Sequential Monte Carlo for Quantum Parameter Estimation]].

---

## References within this paper

- Wiebe, Granade, Ferrie, Cory (2014), arXiv:1309.0876, PRL 112, 190501 — [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|the companion paper]] introducing noiseless QHL; directly extended here
- Granade, Ferrie, Wiebe, Cory (2012), New J. Phys. — [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes|Robust Online Hamiltonian Learning]] (if in vault); precursor SMC approach
- Liu and West (2001) — the Liu–West resampler used in SMC; standard statistical reference
- GRAPE pulse design references — experimental swap gate generation for SC qubits; hardware-specific
- Hentschel & Sanders (2010) — machine learning for quantum metrology; related adaptive experiment design
- Wecker & Svore (2014), LIQUi|⟩ — quantum simulation framework used for numerics

---

## Cross-links

### Paper notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — direct predecessor; noiseless version of this algorithm
- [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes]] — follow-up extending QHL to compressed-sensing sparse models (if in vault)
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — related "learn efficiently without full tomography" theme; complementary approach
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — PAC learning context
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]] — Wiebe early QML work; data fitting vs system identification

### Trick cards
- [[Noise-Robust Bayesian Hamiltonian Learning]]
- [[Superoperator Noise Incorporation for Robust QHL]]
- [[Bayesian Model Selection via SMC Marginal Likelihoods]]
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]

### Subsequent work
- [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes]] — first experimental demonstration of QHL; validates the model selection and variance saturation predictions from this paper
