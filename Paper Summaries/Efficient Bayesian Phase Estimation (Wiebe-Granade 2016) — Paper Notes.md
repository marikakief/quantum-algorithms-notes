> **Source:** Nathan Wiebe and Chris Granade, *Efficient Bayesian Phase Estimation*, arXiv:1508.00869, Phys. Rev. Lett. **117**, 010503 (2016)
> **Links:** [arXiv](https://arxiv.org/abs/1508.00869) · [PRL](https://doi.org/10.1103/PhysRevLett.117.010503)
> **Tags:** #phase-estimation #Bayesian-inference #adaptive #rejection-sampling #Heisenberg-limit #decoherence #RFPE

---

## The computational problem

**Phase estimation:** Given a unitary $U$ with eigenstate $|\phi\rangle$ and eigenvalue $e^{i\phi}$, estimate the eigenphase $\phi$ to precision $\varepsilon$ using adaptive single-qubit measurements.

The standard measurement circuit is iterative: prepare an ancilla $|0\rangle$, apply $H$, controlled-$U^M$, a phase shift $Z(M\theta)$, $H$ again, then measure. The outcome probability is

$$P(0 \mid \phi;\, \theta, M) = \frac{1 + \cos(M(\phi + \theta))}{2}.$$

The design parameters $(M, \theta)$ can be chosen adaptively based on previous outcomes, and the classical inference over $\phi$ is performed via Bayes' rule.

---

## What the paper does

Introduces **Rejection Filtering Phase Estimation (RFPE)**, a lightweight Bayesian phase estimation algorithm that approximates the posterior over $\phi$ as a Gaussian $\mathcal{N}(\mu, \sigma^2)$, updated via rejection sampling rather than maintaining an explicit particle set. Paired with a variant of the [[Particle Guess Heuristic for Adaptive Quantum Metrology|Particle Guess Heuristic (PGH)]] for experiment design, RFPE achieves near-[[Iterative Phase Estimation (Kitaev)|Heisenberg-limited]] scaling in total evolution time while remaining computationally cheap, noise-tolerant, and self-diagnosing.

This is a strong practical contribution. It takes the Bayesian inference machinery from the [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory QHL framework]] and strips it down to the single-parameter phase estimation case, replacing the full [[Sequential Monte Carlo for Quantum Parameter Estimation|Sequential Monte Carlo (SMC)]] machinery with something far cheaper. The result is an algorithm that's genuinely deployable — low memory, low classical computation, graceful degradation under noise.

---

## The algorithm

### RFPE core update

The posterior over $\phi$ is maintained as a Gaussian $\mathcal{N}(\mu, \sigma^2)$. After each measurement with parameters $(M, \theta)$ yielding outcome $E \in \{0, 1\}$:

1. Draw $m$ samples $\phi_j \sim \mathcal{N}(\mu, \sigma^2)$.
2. Accept each $\phi_j$ with probability $P(E \mid \phi_j;\, \theta, M) / \kappa_E$, where $\kappa_E \leq 1$ ensures the acceptance probability stays $\leq 1$.
3. Fit a new Gaussian to the accepted samples: $\mu \leftarrow \overline{\phi}_{\text{accept}}$, $\sigma \leftarrow \sqrt{\mathrm{Var}(\phi_{\text{accept}})}$.

**Why it works:** The accepted samples are drawn from a distribution proportional to $P(E \mid \phi) \cdot \mathcal{N}(\phi \mid \mu, \sigma^2)$, which is the Bayesian posterior (up to normalisation). The Gaussian fit is an approximation — the true posterior is not Gaussian — but for a single cosine likelihood it's a reasonable one, and errors self-correct over multiple updates as the posterior concentrates.

The number of rejection samples $m$ must scale as $({\alpha}/{\delta})^2$ where $\alpha$ is the baseline acceptance probability and $\delta$ is the likelihood variation across samples (see the stability theorem below). In practice, $m \sim 10^3$–$10^4$ works well.

### Experiment design: PGH for phase estimation

The evolution length and phase offset are chosen as:

$$M = \left\lceil \frac{1.25}{\sigma} \right\rceil, \qquad \theta \sim \mathcal{N}(\mu, \sigma^2).$$

The logic: set $M \sim 1/\sigma$ so the phase sensitivity of $\cos(M\phi)$ matches the current posterior width. The factor 1.25 is empirically tuned. This is a simplified version of the [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH from QHL]]: there, two particles are sampled and $t = 1/|x' - x|$; here, $M \approx 1.25/\sigma$ achieves the same effect since the expected inter-particle distance scales as $\sigma$.

### Handling periodicity

Since $\phi$ is periodic on $[0, 2\pi)$, the Gaussian arithmetic mean can fail when the posterior wraps around the branch cut. The paper uses a directional statistics workaround: compute the circular mean and circular standard deviation, or heuristically compute the mean in two shifted frames (unshifted and shifted by $\pi$) and pick the frame with smaller variance.

---

## Key results

### Scaling (empirical)

The median error decays exponentially in the number of experiments:

$$\varepsilon_{\text{median}} \sim e^{-\lambda N}, \qquad \lambda \approx 0.17.$$

This means $O(\log(1/\varepsilon))$ experiments suffice for precision $\varepsilon$. The total evolution time $\sum_k M_k$ scales as $O(1/\varepsilon)$, saturating the Heisenberg limit up to constant factors.

### Comparison with Kitaev's iterative PE

After 150 experiments, RFPE's median error is roughly $10^7 \times$ smaller than [[Iterative Phase Estimation (Kitaev)|Kitaev's method]] with $s = 10$ repetitions per bit. This is the right comparison: both use a single ancilla qubit, but RFPE's adaptive experiment design extracts far more information per measurement.

The paper also compares against ITPE (Svore-Hastings information-theoretic phase estimation). ITPE needs $\sim 5\times$ more $U$-applications for integer-phase cases, and requires ad hoc modifications (such as halving large $M$ values) for non-integer phases, where it can be brittle.

### Decoherence model

Under $T_2$ dephasing, the likelihood becomes:

$$P(0 \mid \phi) = e^{-M/T_2} \frac{1 + \cos(M(\phi - \theta))}{2} + \frac{1 - e^{-M/T_2}}{2}.$$

The PGH is modified to cap $M \leq T_2$:

$$M = \min\!\left(\left\lceil \frac{1.25}{\sigma} \right\rceil,\; T_2\right).$$

Once $\sigma$ shrinks below $1.25/T_2$, further experiments are capped at $M = T_2$ and learning transitions from exponential to polynomial: the median error scales roughly as $N^{-0.6}$, comparable to the $1/\sqrt{N}$ standard quantum limit. This is the correct physical behaviour — you can't beat the decoherence-limited precision without longer coherence times.

### Robustness to unmodelled noise

Simulations with uncharacterised depolarising probability $\gamma$ (the likelihood model used for inference does *not* include $\gamma$) show RFPE still learns, with the exponential decay rate degrading as:

$$\lambda(\gamma) \approx 0.17\, e^{-3.1\gamma}.$$

This is the same graceful degradation pattern as in the [[Noise-Robust Bayesian Hamiltonian Learning|noise-robustness analysis from the QHL papers]]. The Bayesian update structure absorbs moderate unmodelled noise without catastrophic failure.

---

## The restart protocol

RFPE posteriors can have heavy tails: occasional large errors even when the median is tiny. The paper proposes a restart/test strategy to detect and recover from failures:

1. After each update, with probability $e^{-M/T_2}$, perform a diagnostic experiment: set $\theta = \mu$, $M = \tau/\sigma$ with $\tau < 1$ (e.g., $\tau = 0.1$).
2. If the diagnostic outcome is 1 (the low-probability result if the current prior is correct), this is evidence the posterior is wrong — re-prepare the initial state and reset $\sigma$ to the initial width.
3. Once restarted, if $\sigma$ later shrinks below a promised spectral gap $\Delta$, snap to the nearest known eigenvalue and resume.

The restart test can be formalised via a Bayes factor (Appendix D): for typical parameters, the Bayes factor for "prior wrong" given outcome 1 can exceed $10^3$, providing strong evidence for resetting. The mean error with restarting drops from $\sim 0.05$ rad to $\sim 10^{-6}$ rad in the examples shown.

---

## Stability theorem (Appendix C)

When the likelihood is nearly flat across particles — $P(E \mid \phi_j) = \alpha + \delta_j$ with $|\delta_j| \leq \delta$ and $\alpha \geq 10\delta$ — the rejection filter becomes unstable unless the sample count satisfies $m = \Omega(\alpha^2/\delta^2)$. The posterior mean shift from a single datum is $O(\sigma/\sqrt{m})$, which can overwhelm the true Bayesian shift when $m$ is too small.

**Practical consequence:** When likelihoods are very flat (e.g., near the midpoint $\phi + \theta \approx \pi/(2M)$ where $\cos(M(\phi + \theta)) \approx 0$), RFPE needs more samples per update. For most PGH-selected experiments this isn't an issue — the heuristic specifically picks $(M, \theta)$ to make likelihoods informative.

---

## Comparison with prior work

| Method | Ancilla qubits | Experiments for $\varepsilon$ | Total evolution time | Noise tolerance | Classical cost |
|--------|:-:|:-:|:-:|:-:|:-:|
| Textbook QPE | $O(\log 1/\varepsilon)$ | 1 | $O(1/\varepsilon)$ | Low | QFT circuit |
| [[Iterative Phase Estimation (Kitaev)\|Kitaev IPEA]] | 1 | $O(\log(1/\varepsilon) / \delta^2)$ | $O(1/\varepsilon)$ | Low | Feed-forward |
| [[Sequential Monte Carlo for Quantum Parameter Estimation\|SMC-based PE]] | 1 | $O(\log 1/\varepsilon)$ | $O(1/\varepsilon)$ | Moderate | $O(N_p)$ per step |
| ITPE (Svore-Hastings) | 1 | $O(\log 1/\varepsilon)$ | $O(1/\varepsilon)$ | Low | Low |
| **RFPE (this paper)** | **1** | $O(\log 1/\varepsilon)$ | $O(1/\varepsilon)$ | **High** | $O(m)$ per step |

RFPE's main advantages over Kitaev are adaptive experiment design and noise tolerance. Over full SMC, the advantage is computational: RFPE's Gaussian parametrisation avoids maintaining $O(N_p)$ weighted particles and the associated resampling machinery. The trade-off is that RFPE's Gaussian assumption can't represent multimodal posteriors, which SMC handles naturally.

---

## Limits / caveats

- The Gaussian posterior approximation breaks down for multimodal distributions. In phase estimation this mainly matters at the start (wide prior, periodic likelihood), but the restart protocol provides a safety net.
- The empirical constant $\lambda \approx 0.17$ has no rigorous proof — the Heisenberg scaling is demonstrated numerically, not proven analytically. This is a gap worth noting.
- The number of rejection samples $m$ can blow up when likelihoods are flat (stability theorem). In the worst case, RFPE degenerates to uninformed random sampling.
- Tail behaviour is worse than SMC without the restart protocol. The restart protocol helps but adds overhead (extra diagnostic experiments).
- Comparison with ITPE is somewhat unfair: the non-integer phase case where ITPE struggles may be fixable with better classical post-processing, and the $5\times$ gap is not orders of magnitude.

---

## Reusable ideas

1. [[Rejection Filtering for Lightweight Bayesian Updates]] — replace a full particle filter with Gaussian + rejection sampling for single-parameter inference
2. [[PGH Adaptation for Single-Parameter Phase Estimation]] — simplify the two-particle PGH to $M = \lceil c/\sigma \rceil$ when the parameter space is one-dimensional
3. [[Bayesian Restart Protocol for Phase Estimation]] — detect posterior failure via cheap diagnostic measurements and Bayes factor, reset and resume

---

## References within this paper

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — foundational phase estimation; the bit-by-bit method that RFPE improves upon
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve-Ekert-Macchiavello-Mosca (1998)]] — standard QPE circuit formulation
- [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes|Granade-Ferrie-Wiebe-Cory (2012)]] — SMC Bayesian inference for quantum parameter estimation; the direct precursor
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory (2014)]] — QHL framework with IQLE + PGH; RFPE distills the PGH for single-parameter use
- Svore, Hastings, Freedman, *Faster Phase Estimation*, arXiv:1304.0741 (2014) — ITPE, the main comparison target; no vault note
- Ferrie, Granade, Cory, *How to best sample a periodic probability distribution*, arXiv:1110.3067 (2013) — optimal Bayesian experiment design for phase
- Doucet, de Freitas, Gordon, *Sequential Monte Carlo Methods in Practice*, Springer (2001) — the SMC machinery underlying the approach

---

## Cross-links

### Paper notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — parent framework; RFPE is the QHL machinery stripped to one parameter
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — noise-robustness analysis that parallels RFPE's unmodelled-noise results
- [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes]] — extended QHL; shares the Bayesian/PGH framework
- [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes]] — SMC precursor
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — foundational PE that RFPE improves upon
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — uses RFPE for QPE cost estimation ($M(\varepsilon) \approx 2.3/\varepsilon$)
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]] — alternative Heisenberg-limited approach for early FT; spectral CDF instead of Bayesian
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]] — different context, same author cluster

### Trick cards
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]] — the experiment design heuristic adapted here for single-parameter use
- [[Sequential Monte Carlo for Quantum Parameter Estimation]] — the heavier machinery that RFPE replaces
- [[Iterative Phase Estimation (Kitaev)]] — the deterministic bit-by-bit approach that RFPE outperforms
- [[Noise-Robust Bayesian Hamiltonian Learning]] — same graceful-degradation pattern under depolarising noise
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]] — QHL likelihood trick; not needed for single-parameter PE but shares the philosophy
- [[Majority Vote Bit Refinement for Phase Estimation]] — alternative error-reduction strategy for iterative PE
- [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]] — complementary trick that halves the unitary cost, used with RFPE in resource estimates
- [[Rejection Filtering for Lightweight Bayesian Updates]] — new trick card from this paper
- [[PGH Adaptation for Single-Parameter Phase Estimation]] — new trick card from this paper
- [[Bayesian Restart Protocol for Phase Estimation]] — new trick card from this paper
