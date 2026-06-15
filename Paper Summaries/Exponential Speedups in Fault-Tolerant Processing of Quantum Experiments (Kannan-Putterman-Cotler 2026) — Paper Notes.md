> **Source:** Ishaan Kannan, Harald Putterman, and Jordan Cotler, *Exponential speedups in fault-tolerant processing of quantum experiments*, arXiv:2605.02057 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2605.02057) · [PDF](https://arxiv.org/pdf/2605.02057)
> **Tags:** #fault-tolerance #quantum-learning #classical-shadows #quantum-tomography #surface-code #lower-bounds #quantum-metrology

---

> **Status / scope:** 2026 arXiv v1. The exponential separations are upload-first fault-tolerant quantum processing versus unuploaded/raw noisy quantum processing of experimental states. They are not classical-vs-quantum separations.

## The computational problem

The paper studies learning tasks where the unknown quantum state is produced by a **physical experiment**, not by a fault-tolerant oracle. The learner has a fault-tolerant processor, but the experimental state initially lives in unprotected physical degrees of freedom. The central architectural question is:

> Should the learner process the raw experimental state directly, accepting physical noise during the learning circuit, or first inject the state into an error-correcting code and only then run the learning algorithm fault-tolerantly?

The paper compares two sample complexities.

| Mode | What happens to each copy of $\rho$ | Sample complexity |
|---|---|---|
| **Raw processing** | The learner acts directly on the unencoded state; coherent preprocessing suffers physical depolarising noise of strength $\lambda$. | $N_{\rm raw}$ |
| **Uploaded processing** | The state is transduced into memory, injected into a high-distance surface-code patch, pays a one-time effective input noise $\lambda'$, then all later processing is logical. | $N_{\rm inj}$ |

Notation used below:

| Symbol | Meaning |
|---|---|
| $\lambda$ | raw physical depolarising noise strength in the learning lower bounds |
| $\lambda'$ or $\eta$ | effective one-time input noise after upload, depending on the theorem section |
| $p$ | below-threshold physical circuit-noise rate in the surface-code upload theorem |
| $N_{{\rm inj},i}$ | initial marginal input channel, generally $(1-q_i)\operatorname{Id}+q_iR_i$ |
| $D_\lambda$ | depolarising channel used after twirling or in the lower-bound model |

The two concrete learning tasks are:

1. **Noisy shallow classical shadows.** Estimate a $k$-local Pauli observable $P_k$ on an $n$-qubit state $\rho$ using randomized brickwork-circuit preprocessing. This connects directly to [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|classical shadows]], [[Classical Shadow Channel Inversion]], and [[Shadow Norm as Measurement-Observable Match]].

2. **Third moments and cubic observables.** Estimate quantities such as

$$
\operatorname{tr}(\rho^3), \qquad \operatorname{tr}(O\rho^3),
$$

which require coherent three-copy processing. This includes the cyclic-permutation / generalized-SWAP primitive behind purity and higher-moment estimation, and is related to [[Swap Test for State Comparison]] and [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|quantum PCA]].

The paper also frames this complexity-theoretically using noisy oracle access. For a state-preparation oracle $O:1\mapsto \rho_O$, define the noisy oracle

$$
N_{\lambda}(O)=D_{\lambda}^{\otimes n}\circ O,
$$

where $D_\lambda$ is single-qubit depolarising noise. Uploading collapses repeated exposure to unprotected noise into one noisy oracle call.

## What the paper does

This is a good paper. The main point is not “fault tolerance helps because noise is bad”; that would be too vague. The specific claim is sharper: **for experimentally supplied quantum states, uploading first can be exponentially better than doing even optimised raw coherent quantum processing before encoding.**

The construction is a surface-code state-injection / code-growth gadget that maps an arbitrary unknown physical qubit into a distance-$d$ logical patch. The important guarantee is that all the mess from the physical interface is equivalent to a one-time input channel, with residual logical failure suppressed by increasing $d$.

Then they prove separations for two learning primitives:

- noisy shallow shadows, relative to unuploaded adaptive noisy strategies: $N_{\rm raw}/N_{\rm inj}\ge \exp(\Omega(\lambda k))$;
- third moments / cubic observables, relative to unuploaded adaptive noisy strategies: $N_{\rm raw}/N_{\rm inj}\ge \exp(\Omega(\lambda n))$.

My assessment: the technical core is the lower-bound machinery, not the surface-code gadget. The surface-code uploading theorem is useful architecture, but the genuinely portable idea is the **Heisenberg learning tree**: push the adaptive noisy learning process into the Heisenberg picture and bound a fixed distinguishability operator. That feels reusable beyond these two tasks.

## The algorithm / construction

### 1. Quantum uploading

The upload pipeline has two stages.

```text
physical experiment
      │
      ▼
transduction into memory      experiment-specific, still physical
      │
      ▼
surface-code injection        generic code-growth gadget
      │
      ▼
logical encoded state         processed fault-tolerantly
```

The generic injection step is captured by [[Quantum Uploading as One-Time Input Noise]]. For each physical qubit:

1. Start with the unknown physical input qubit.
2. Attach the surrounding qubits needed for a small rotated surface-code patch.
3. Initialise new qubits along the planar diagonal in $|0\rangle$ or $|+\rangle$, depending on the stabilizer sector.
4. Measure stabilizers for $T\ge d$ rounds while growing the patch to distance $d$.
5. Decode syndrome data using a minimum-weight decoder.
6. Interpret certain growth faults as an effective logical input channel; subsequent residual faults are handled by ordinary fault-tolerant gadgets.

The formal version is Theorem 9 in the supplement. For a Clifford+$T$ circuit $C$ of width $W$ and depth $D$ with $l$ arbitrary physical input qubits in state $\rho$, below-threshold local-stochastic noise admits a noisy fault-tolerant implementation $\widetilde C$ such that

$$
\frac12\left\|\operatorname{dec}\circ \widetilde C(\rho)-C\bigl(N_{\rm inj}^{l}(\rho)\bigr)\right\|_1\le \epsilon.
$$

If the input-gadget noise is independent, then

$$
N_{\rm inj}^{l}=\bigotimes_{i=1}^l N_{{\rm inj},i}.
$$

Each marginal channel has the form

$$
N_{{\rm inj},i}=(1-q_i)\operatorname{Id}+q_i R_i,
\qquad q_i\le c_{\rm in}p,
$$

where $p$ is the physical circuit-noise rate and $R_i$ is some CPTP map. Applying independent logical Clifford twirls turns this into a local depolarising input channel with strength $\lambda_*=O(p)$.

### 2. Uploaded oracle reduction

The uploaded state-preparation oracle result is the paper's noisy-oracle equivalence:

$$
\mathrm{NBQP}^{O}=\mathrm{BQP}^{N_{\lambda_*}(O)}
$$

Here $\mathrm{NBQP}^{O}$ is the paper-specific class of bounded-error quantum computations with noisy physical access to the oracle $O$. The equivalence is for state-preparation oracles, up to bounded-error equivalence and below-threshold physical noise. Intuitively:

- without uploading, the learner keeps touching the physical state and pays noise repeatedly;
- with uploading, the physical interface is paid once, then ordinary fault-tolerant computation takes over.

This reframes the noisy-learning no-go story from Cotler--Gong--Kannan: the unavoidable gap is not between ideal BQP learning and all real devices, but between **unuploaded** noisy access and **uploaded** noisy input access.

### 3. Third-moment protocol

For three copies, the basic observable is the cyclic permutation

$$
C=\pi_{123}, \qquad H=\frac{C+C^{-1}}{2},
$$

with

$$
\operatorname{tr}(H\rho^{\otimes 3})=\operatorname{tr}(\rho^3).
$$

If the uploaded input has one-time depolarising noise $D_{\lambda'}^{\otimes n}$ and the later three-copy circuit is logical, the learner can either:

- run the ordinary three-copy cycle test on noisy uploaded copies for the promise problem; or
- use [[Permutation-Observable Noise Deconvolution for Multi-Copy Moments]] to estimate $\operatorname{tr}(\rho^3)$ itself by measuring a deconvolved observable.

For the generic estimator, let $a=1-\lambda'$ and define the inverse single-qubit channel $I_{\lambda'}$ on Pauli operators by

$$
I_{\lambda'}[P]=
\begin{cases}
(1-\lambda')^{-1}P, & P\ne I,\\
P, & P=I.
\end{cases}
$$

Then measure

$$
H_{\lambda'}=(I_{\lambda'}^{\otimes n})^{\otimes 3}[H]
$$

on the three noisy uploaded copies. This is an unbiased estimator of $\operatorname{tr}(\rho^3)$, with variance cost that grows as the inverse-noise factors grow.

### 4. Uploaded shallow shadows

The shadow protocol after uploading is standard at the logical level:

1. Upload $\rho$ into the code, giving

$$
\rho_{\rm log}=U_{\rm enc}D_{\eta}^{\otimes n}(\rho)U_{\rm enc}^{\dagger}.
$$

2. Run a fault-tolerant implementation of the noiseless brickwork shallow-shadow ensemble at depth $d=\Theta(\log k)$.
3. Estimate the logical observable $\overline P_k=U_{\rm enc}P_kU_{\rm enc}^{\dagger}$.
4. Rescale by $(1-\eta)^{-k}$, since

$$
\operatorname{tr}(\overline P_k\rho_{\rm log})=(1-\eta)^k\operatorname{tr}(P_k\rho).
$$

The important design pattern is [[Upload-Then-Randomize for Noisy Shadows]]: do the randomized basis-spreading part after the state is protected, not while it is still a fragile physical object.

## Key results

### Surface-code uploading

Informally, for every distance $d$ there is a depth-$O(d)$ surface-code uploading procedure such that

$$
\rho\longmapsto V_d^{\otimes n}D_{\lambda}^{\otimes n}(\rho)(V_d^{\dagger})^{\otimes n},
\qquad \lambda=\Theta(p),
$$

up to logical error exponentially small in $d$ and harmless residual deviations handled by later fault-tolerant gadgets.

The formal theorem is slightly more careful: the input channel is initially arbitrary of the form $(1-q)\operatorname{Id}+qR$ with $q=O(p)$, then Clifford twirling makes it depolarising for the later learning analysis.

### Third-moment lower bound without uploading

The hard ensembles are

$$
E_p=\left\{\frac{I}{2^{n+1}}+\frac{|\psi_1\rangle\!\langle\psi_1|+|\psi_2\rangle\!\langle\psi_2|}{4}\right\},
$$

and

$$
E_q=\left\{\frac{I}{2^{n+1}}+\frac{|\psi_1\rangle\!\langle\psi_1|}{3}
+\frac{|\psi_2\rangle\!\langle\psi_2|+|\psi_3\rangle\!\langle\psi_3|}{12}\right\},
$$

with the $|\psi_j\rangle$ Haar random. The two ensembles are designed to be hard to distinguish under noisy three-copy processing while having a constant gap in $\operatorname{tr}(\rho^3)$.

Theorem 10 states that any $\lambda$-noisy algorithm with query access to $D_{\lambda}^{\otimes n}[\rho]^{\otimes 3}$ requires

$$
\Omega\!\left(
\min\!\left\{
2^{n/2},
\frac{(2^n+1)^2(2^n+2)^2}{R_n(\eta)}
\right\}
\right)
$$

experiments to solve the distinguishing task, where $\eta=1-\lambda$ and

$$
R_n(\eta)=2(1+9\eta^6+6\eta^{10})^n
+2(1+9\eta^6-6\eta^{10})^n
-12(1+3\eta^6)^n+8.
$$

For small constant $\lambda$, this scales as

$$
\Omega\!\left(
\left(\frac{16}{1+9(1-\lambda)^6+6(1-\lambda)^{10}}\right)^n
\right),
$$

until the independent $2^{n/2}$ cap becomes tighter.

The same lower-bound machinery gives cubic-observable lower bounds for $\operatorname{tr}(O\rho^3)$. In particular, Corollary 10.2 shows that for any $1\le k\le n$ there exists a nontrivial $k$-local observable $O$ for which the same exponential-in-$n$ lower bound applies.

### Third-moment upper bound with uploaded copies

Let $a=1-\lambda'$. With uploaded noisy copies and no further circuit noise, the generic deconvolved estimator uses

$$
O\!\left(
\frac{1}{\epsilon^2}
\left(\frac{1-6a^{-2}+9a^{-4}+12a^{-6}}{16}\right)^n
\log\frac1\delta
\right)
$$

experiments to estimate $\operatorname{tr}(\rho^3)$ to additive error $\epsilon$ with failure probability $\delta$.

This expression is specific to the paper's 2026 theorem and deconvolution convention. The robust takeaway is qualitative: inverse-noise deconvolution is unbiased, but its variance can grow exponentially with system size as inverse powers of $1-\lambda'$ accumulate.

For the particular hard distinguishing task, the simpler ordinary three-copy cycle test on $D_{\lambda'}^{\otimes n}[\rho]$ uses

$$
O\!\left(
\left(\frac{16}{1+9a^2+6a^3}\right)^{2n}
\log\frac1\delta
\right)
$$

experiments.

Combining lower and upper bounds, Theorem 12 gives

$$
\frac{N_{\rm raw}}{N_{\rm inj}}\ge \Omega(\exp(\lambda n))
$$

for small $\lambda$ whenever

$$
\lambda' < \frac{19}{12}\lambda.
$$

So uploading can tolerate a larger constant noise rate than raw processing and still win exponentially.

### Shallow-shadow separation

For noisy brickwork shallow shadows, Lemma 18 gives the raw lower bound

$$
N\ge \Omega\!\left(
\frac{1}{\omega_{\lambda,d}(P_k)(1-\lambda)^{2k}\epsilon^2}
\right),
$$

where $\omega_{\lambda,d}(P_k)$ is the noisy Pauli shadow weight of the $k$-local Pauli under the depth-$d$ brickwork ensemble.

Lemma 20 gives the uploaded upper bound

$$
N=O\!\left(
\frac{3^{3k/4}\operatorname{poly}(k)}{(1-\eta)^{2k}\epsilon^2}
\log\frac1\delta
\right).
$$

Theorem 13 then gives

$$
\frac{N_{\rm raw}}{N_{\rm inj}}\ge \exp(\Omega(\lambda k))
$$

for small $\lambda$ whenever

$$
\lambda' < \frac54\lambda.
$$

The general condition is

$$
\lambda' <
\begin{cases}
1-(1-\lambda)e^{-\lambda/4}, & \lambda\le \frac12\log 3,\\
1-(1-\lambda)3^{-1/8}, & \lambda>\frac12\log 3.
\end{cases}
$$

## Comparison with prior work

| Work | What it established | Relation here |
|---|---|---|
| [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang--Kueng--Preskill 2020]] | Classical shadows estimate many observables with sample cost controlled by shadow norm. | This paper asks what happens when the randomized preprocessing itself is noisy because the state came from a physical experiment. |
| [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Lloyd--Mohseni--Rebentrost 2014]] | Density matrix exponentiation and qPCA from multiple copies of $\rho$. | The exoplanet example uses qPCA / density matrix exponentiation-style processing, but protected by uploading. |
| [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes|Motlagh--Wiebe 2023]] | Generalized QSP implements flexible polynomial transformations. | The astronomical imaging pipeline uses QSP-style eigenvalue filtering after storing photons. |
| [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes|Aharonov--Ben-Or 2008]] | Threshold theorem: long quantum computation is possible below constant physical noise. | This paper adds an input-boundary question: how do unencoded experimental states enter the protected computation? |
| [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill 2005]] | High-threshold fault-tolerant architecture via postselected error-detecting codes. | Background for treating fault tolerance as a practical processing layer, though the present upload gadget is surface-code based. |
| Cotler--Gong--Kannan 2026 | Noisy quantum learning theory: unprotected noise can erase ideal quantum learning advantages. | This paper answers by uploading: repeated exposure to noise becomes a one-time input channel. |
| Ye--Liu--Deng 2025 | One extra replica can give exponential advantage for nonlinear-state-property estimation. | This paper shows that noise can destroy that advantage unless the replicas are uploaded first. |
| Mokeev--Saif--Lukin--Borregaard 2025 | Quantum PCA for exoplanet imaging. | Used as the numerical case study: uploaded photons beat raw noisy processing by orders of magnitude in shots. |

## Limits / caveats

- **Transduction is assumed, not solved.** The theory needs a physical interface that maps Nature's state into memory with only constant error. If transduction itself has exponentially bad fidelity or destroys the relevant mode structure, uploading cannot rescue the task.

- **The separations are architectural.** Both sides are quantum protocols. This is not a classical-vs-quantum speedup; it is upload-first fault-tolerant processing versus raw noisy quantum processing.

- **The noise model is stylised.** The main separations use local depolarising noise. The surface-code theorem is phrased for local-stochastic circuit noise, but the learning lower bounds are tailored to depolarising contraction in the Pauli basis.

- **The constants matter.** The clean statements allow injection noise larger than raw noise only by a constant factor: roughly $19/12$ for third moments and $5/4$ for shallow shadows in the small-$\lambda$ regime.

- **The uploading gadget is surface-code specific.** The architectural idea should generalise, but the proof uses rotated surface-code growth, syndrome rounds, spacetime avoiding sets, and weight-enumerator bounds.

- **The strongest lower bounds are worst-case.** The hard third-moment ensembles are random-state constructions. They are good for proving separations, not necessarily the distribution one would see in a concrete lab experiment.

- **The exoplanet numerics are illustrative.** Useful and suggestive, but still a model pipeline. The empirical claim depends on the chosen optical-state model, noise placement, QSP optimisation, and photon budget assumptions.

## Reusable ideas

1. [[Quantum Uploading as One-Time Input Noise]] — convert an unprotected experimental-state interface into a one-time effective input channel, then run the rest of the learning circuit logically.

2. [[Heisenberg Learning Tree Lower Bounds]] — prove noisy adaptive learning lower bounds by pushing the protocol into a Heisenberg-picture distinguishability operator and bounding likelihood ratios nodewise.

3. [[Permutation-Observable Noise Deconvolution for Multi-Copy Moments]] — estimate nonlinear moments such as $\operatorname{tr}(\rho^3)$ from noisy copies by applying the inverse noise channel to the permutation observable rather than trying to denoise the state.

4. [[Upload-Then-Randomize for Noisy Shadows]] — for randomized measurement protocols, protect the state before applying basis-spreading random circuits; otherwise the randomizer also becomes the noise amplifier.

## References within this paper

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang, Kueng, and Preskill 2020]] — classical shadows, the randomized-measurement primitive used in the shallow-shadow separation.
- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Lloyd, Mohseni, and Rebentrost 2014]] — qPCA and density matrix exponentiation; background for the imaging pipeline.
- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes|Motlagh and Wiebe 2023]] — QSP-style polynomial filtering used in the exoplanet imaging discussion.
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev / surface-code lineage]] and [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes|Aharonov--Ben-Or]] — conceptual fault-tolerance background.
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill 2005]] — high-threshold fault-tolerance context.
- Cotler, Gong, and Kannan 2026, *Noisy quantum learning theory*, arXiv:2512.10929 — the no-go result this paper sharpens.
- Ye, Liu, and Deng 2025, *Exponential advantage from one more replica in estimating nonlinear properties of quantum states*, arXiv:2509.24000 — source of the many-vs-many third-moment hard ensembles.
- Mokeev, Saif, Lukin, and Borregaard 2025, *Enhancing optical imaging via quantum computation*, arXiv:2509.09465 — exoplanet imaging application.
- He, Nguyen, and Pattison 2025, arXiv:2508.08246 — composable gadget / avoiding-set framework used in the surface-code upload proof.

## Cross-links

### Paper notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]]
- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]

### Trick cards

- [[Quantum Uploading as One-Time Input Noise]]
- [[Heisenberg Learning Tree Lower Bounds]]
- [[Permutation-Observable Noise Deconvolution for Multi-Copy Moments]]
- [[Upload-Then-Randomize for Noisy Shadows]]
- [[Classical Shadow Channel Inversion]]
- [[Shadow Norm as Measurement-Observable Match]]
- [[Median-of-Means Shadow Prediction]]
- [[Density Matrix Exponentiation via Partial Swap]]
- [[Swap Test for State Comparison]]
- [[Sparse Fault Path Analysis]]
- [[Thresholded Low-Degree Heisenberg Learning]]
