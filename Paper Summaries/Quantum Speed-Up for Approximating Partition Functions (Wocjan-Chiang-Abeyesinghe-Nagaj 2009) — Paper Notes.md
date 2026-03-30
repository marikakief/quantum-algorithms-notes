> **Source:** Pawel Wocjan, Chen-Fu Chiang, Anura Abeyesinghe, Daniel Nagaj, *Quantum speed-up for approximating partition functions*, Phys. Rev. A **80**, 022340 (2009); arXiv:0811.0596  
> **Links:** [arXiv](https://arxiv.org/abs/0811.0596) · [PRA](https://doi.org/10.1103/PhysRevA.80.022340)  
> **Tags:** #partition-function #quantum-walk #phase-estimation #amplitude-estimation #simulated-annealing #FPRAS #Markov-chain

---

## The computational problem

Estimate the Gibbs partition function $Z(T_F) = \sum_{\sigma \in \Omega} e^{-E(\sigma)/kT_F}$ of a classical system at temperature $T_F$, given access to rapidly-mixing Markov chains whose stationary distributions are the Boltzmann distributions at intermediate temperatures.

Classical FPRAS (fully polynomial randomised approximation scheme) for this problem: choose a cooling schedule $T_0 \geq T_1 \geq \cdots \geq T_\ell = T_F$, express $Z(T_F)$ as a telescoping product of ratios $\alpha_i = Z_{i+1}/Z_i$, estimate each ratio by sampling from $\pi_i$ (Boltzmann at $T_i$), compose the estimates. Classical cost: $\tilde{O}(\ell^2 / (\delta \cdot \varepsilon^2))$ Markov chain steps, where $\delta$ is the spectral gap.

**Question:** Can quantum computers speed this up?

---

## What the paper does

Yes. Gives a quantum FPRAS with cost $\tilde{O}(\ell^2 / (\sqrt{\delta} \cdot \varepsilon))$ quantum walk invocations — a quadratic speedup in *both* the spectral gap $\delta$ and the accuracy $\varepsilon$. These two reductions are linked and cannot be achieved separately.

This is one of the earlier results showing quantum speedups for *estimation* (not search) problems via quantum walks + phase estimation. It predates [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro's 2015 framework]], which gives a more general and cleaner treatment of quantum Monte Carlo speedups. The core ideas — preparing coherent encodings of distributions, then using phase estimation instead of classical sampling — appear in both, but Montanaro's approach is more modular.

---

## The algorithm / construction

### Step 1: Telescoping product

Express $Z(T_F) = Z_0 \cdot \alpha_0 \cdot \alpha_1 \cdots \alpha_{\ell-1}$, where $Z_0 = |\Omega|$ (partition function at $T_0 = \infty$) and $\alpha_i = Z_{i+1}/Z_i$.

### Step 2: Quantum ratio estimation (perfect case)

For each ratio $\alpha_i$, prepare the coherent encoding (quantum sample):

$$|\pi_i\rangle = \sum_{\sigma \in \Omega} \sqrt{\pi_i(\sigma)} |\sigma\rangle$$

Apply a controlled rotation $V_i$ that encodes the ratio $y_i(\sigma) = e^{-(\beta_{i+1} - \beta_i)E(\sigma)}$ into an ancilla qubit:

$$V_i(|\pi_i\rangle \otimes |0\rangle) = |\psi_i\rangle$$

Then $\langle \psi_i | (I \otimes |0\rangle\langle 0|) | \psi_i \rangle = \alpha_i$.

This is a generalisation of [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|quantum counting]]: estimate the expectation value of a projector on a quantum state by applying [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] / phase estimation on the Grover-like iterate

$$G = (2|\psi_i\rangle\langle\psi_i| - I)(2P - I)$$

where $P = I \otimes |0\rangle\langle 0|$. Phase estimation on $G$ yields $\theta$ with $\cos\theta = 2\alpha_i - 1$, giving $\alpha_i$ to precision $\varepsilon_{\text{pe}}$ using $O(1/\varepsilon_{\text{pe}})$ controlled-$G$ operations — a quadratic improvement over the $O(1/\varepsilon_{\text{pe}}^2)$ classical samples needed.

### Step 3: Composition

Apply Lemma 3 (composition of ratio estimates): set $\varepsilon_{\text{pe}} = \varepsilon/(2\ell)$ for each ratio, boost success probability to $1 - 1/(4\ell)$ via the powering lemma, then multiply all $\ell$ estimates. Overall success probability $\geq 3/4$.

### Step 4: Approximate state preparation (full algorithm)

Drop the assumption of perfect $|\pi_i\rangle$ preparation. Use [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walks]]: given the classical Markov chain $P_i$ with spectral gap $\delta$, the quantum walk $W(P_i)$ has phase gap $\Theta(\sqrt{\delta})$. Prepare $|\pi_i\rangle$ approximately by driving $|\pi_0\rangle$ toward $|\pi_i\rangle$ through intermediate states, using Grover's $\pi/3$-fixed-point search at each step. Cost: $O(\ell / \sqrt{\delta} \cdot \log^2 \ell)$ walk invocations per state.

Approximate reflections $\tilde{R}_i \approx 2|\pi_i\rangle\langle\pi_i| - I$ via phase estimation on the quantum walk: cost $O(1/(\sqrt{\delta}) \cdot \log(1/\varepsilon_R))$ per reflection.

---

## Key results

**Theorem 1 (Main result).** A classical FPRAS for partition functions using simulated annealing with $\ell$ cooling steps, Markov chains with spectral gap $\delta$, and accuracy $\varepsilon$ can be quantised to use

$$\tilde{O}\!\left(\frac{\ell^2}{\sqrt{\delta} \cdot \varepsilon}\right)$$

controlled quantum walk invocations.

| | Classical FPRAS | Quantum FPRAS |
|---|---|---|
| Spectral gap dependence | $1/\delta$ | $1/\sqrt{\delta}$ |
| Accuracy dependence | $1/\varepsilon^2$ | $1/\varepsilon$ |
| Total cost | $\tilde{O}(\ell^2/(\delta \varepsilon^2))$ | $\tilde{O}(\ell^2/(\sqrt{\delta}\varepsilon))$ |

**Note on cooling schedule:** The paper assumes non-adaptive cooling schedules where $\alpha_i \geq 1/2$. The extension to Chebyshev cooling schedules (which can be shorter but have weaker guarantees on $\alpha_i$) is left as an open question.

---

## Comparison with prior work

| Paper | What it does | Relation |
|---|---|---|
| [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes\|Szegedy (2004)]] | Quantum walk from classical Markov chain; $\sqrt{\delta}$ phase gap | Foundation — this paper uses Szegedy walks as subroutine |
| Wocjan & Abeyesinghe (2008), arXiv:0804.4259 | Quantum sampling via fixed-point search on slowly-varying chains | Direct precursor — state preparation method |
| [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes\|Brassard-Høyer-Tapp (1998)]] | Quantum counting via phase estimation on Grover operator | This paper generalises quantum counting to estimate expectations |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes\|Brassard-Høyer-Mosca-Tapp (2002)]] | General amplitude estimation framework | The ratio estimation step is an instance of amplitude estimation |
| [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes\|Montanaro (2015)]] | General quantum Monte Carlo speedup; $\tilde{O}(\sigma/\varepsilon)$ | Supersedes this paper with a cleaner, more general framework |
| [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes\|Mann-Helmuth (2021)]] | Classical FPTAS for quantum partition functions via cluster expansion | Different approach — purely classical, works at high temperature |
| Poulin & Wocjan (2009), arXiv:0905.2199 | Sampling from quantum Gibbs states | Extends to quantum (not just classical) Hamiltonians |

---

## Limits / caveats

- Requires rapidly-mixing Markov chains whose construction is system-specific (Ising, Potts, etc.). If the classical FPRAS doesn't exist, neither does this quantum version.
- The speedup is relative to the classical FPRAS that uses the *same* cooling schedule. If the cooling schedule is suboptimal, so is the quantum algorithm.
- The two quadratic speedups ($\delta \to \sqrt{\delta}$ and $\varepsilon^2 \to \varepsilon$) are coupled — you can't get one without the other. This is because the $\varepsilon$ speedup requires coherent quantum samples, and preparing those costs $O(1/\sqrt{\delta})$ per step.
- The paper handles classical Hamiltonians only. Quantum partition functions $\text{Tr}(e^{-\beta H})$ for quantum $H$ require different techniques (e.g., [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|quantum Metropolis sampling]]).
- Extension to Chebyshev cooling schedules (potentially shorter $\ell$) is not resolved.

---

## Reusable ideas

1. [[Coherent Encoding of Classical Distributions for Quantum Speedup]] — prepare $|\pi\rangle = \sum \sqrt{\pi(\sigma)}|\sigma\rangle$ instead of sampling from $\pi$; enables phase estimation on expectations rather than classical averaging
2. [[Quantum Ratio Estimation via Phase Estimation]] — estimate $\langle \psi | P | \psi \rangle$ to additive precision $\varepsilon$ using $O(1/\varepsilon)$ queries to the Grover-like iterate, quadratically faster than classical sampling

---

## References within this paper

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — the quantum walk framework used throughout
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] — quantum counting, which this paper generalises
- Wocjan & Abeyesinghe (2008), arXiv:0804.4259 — quantum sample preparation from slowly-varying Markov chains (the companion paper)
- Grover (2005) — $\pi/3$-fixed-point search used in state preparation
- Jerrum & Sinclair (1993) — classical FPRAS for Ising partition function
- Jerrum, Sinclair & Vigoda (2001) — classical FPRAS for the permanent
- Stefankovic, Vempala & Vigoda (2007) — Chebyshev cooling schedules

---

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]

### Trick cards
- [[Coherent Encoding of Classical Distributions for Quantum Speedup]]
- [[Quantum Ratio Estimation via Phase Estimation]]
- [[Amplitude Estimation via Phase Estimation on Q]]
