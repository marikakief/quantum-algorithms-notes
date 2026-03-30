> **Source:** Mária Kieferová and Nathan Wiebe, *Tomography and generative training with quantum Boltzmann machines*, Phys. Rev. A **96**, 062327 (2017); arXiv:1612.05204  
> **Links:** [arXiv](https://arxiv.org/abs/1612.05204) · [PRA](https://doi.org/10.1103/PhysRevA.96.062327)  
> **Tags:** #quantum-machine-learning #Boltzmann-machine #tomography #Gibbs-state #relative-entropy #generative-model #fermionic

---

## The computational problem

Train a quantum Boltzmann machine (QBM) — a quantum generalisation of the classical Boltzmann machine where the model distribution is a Gibbs state $e^{-H}/Z$ of a quantum Hamiltonian $H$. Two goals:

1. **Generative modelling:** find $H$ such that the thermal state $e^{-H}/Z$ best explains observed data
2. **Quantum state tomography:** given copies of an unknown state $\rho$, learn $H$ such that $e^{-H}/Z \approx \rho$, providing both a compact description *and* a generative procedure for producing new copies

The central challenge: for quantum $H$, the Hamiltonian doesn't commute with its parametric derivatives, so the classical gradient expressions (which assume commutativity) break down.

---

## What the paper does

Provides two new training methods for quantum Boltzmann machines that fix limitations of prior work (Amin et al. 2016):

1. **POVM-based Golden-Thompson training:** generalises the classical log-likelihood objective to allow non-diagonal POVM elements as training data, which enables learning quantum (off-diagonal) Hamiltonian terms. Prior work could only learn classical terms from classical data.

2. **Relative entropy training:** minimises $S(\rho \| e^{-H}/Z)$ directly, with exact gradients $\nabla_\theta O = -\text{Tr}[\rho \partial_\theta H] + \text{Tr}[e^{-H}\partial_\theta H]/Z$. No approximations needed (unlike Golden-Thompson). This simultaneously gives a tomographic reconstruction: if training converges, $e^{-H}/Z \approx \rho$.

The Hamiltonian model is explicitly fermionic (non-stoquastic), defeating quantum Monte Carlo simulation:

$$H = \sum_p h_p(a_p + a_p^\dagger) + \frac{1}{2}\sum_{pq} h_{pq}(a_p^\dagger a_q + a_q^\dagger a_p) + \frac{1}{2}\sum_{pqrs} h_{pqrs}(a_p^\dagger a_q^\dagger a_r a_s + \text{h.c.})$$

This reduces to the classical Ising model when quantum terms vanish, and is expressive enough to represent any density matrix (when the term set is complete).

---

## The algorithm / construction

### POVM-based training (Golden-Thompson)

**Objective:** maximise the average log-likelihood

$$O_\Lambda(H; \lambda) = \sum_v P_v \log\left(\frac{\text{Tr}[\Lambda_v e^{-H}]}{\text{Tr}[e^{-H}]}\right) - \frac{\lambda}{2}\|h_Q\|^2$$

where $\Lambda = \{\Lambda_v\}$ is a POVM on the visible subsystem, $P_v$ is the empirical distribution, and the $L_2$ regularisation term penalises unnecessary quantum terms.

**Problem:** derivatives involve $\text{Tr}[\Lambda_v \partial_\theta e^{-H}]$, which by Duhamel's formula has a time-ordered integral that doesn't simplify when $[H, \partial_\theta H] \neq 0$.

**Solution:** use the Golden-Thompson inequality $\text{Tr}[e^{A+B}] \geq \text{Tr}[e^A e^B]$ to bound the objective from below. The gradient of the lower bound becomes:

$$\sum_v P_v \left(-\frac{\text{Tr}[e^{-H_v}\partial_\theta H]}{\text{Tr}[e^{-H_v}]} + \frac{\text{Tr}[e^{-H}\partial_\theta H]}{\text{Tr}[e^{-H}]}\right) - \lambda h_\theta \delta_{H_\theta \in H_Q}$$

where $H_v = H - \log \Lambda_v$. This is a difference of thermal expectations — each computable by sampling from a Gibbs state.

**Key innovation over Amin et al.:** allowing non-diagonal POVM elements $\Lambda_v$ (not just projectors onto computational basis states) means that $\text{Tr}[e^{-H_v}\partial_\theta H]$ can be nonzero for quantum terms $\partial_\theta H$. This is what enables learning the quantum part of the Hamiltonian from measurement data.

### Relative entropy training

**Objective:** minimise $S(\rho \| e^{-H}/Z) = \text{Tr}[\rho \log \rho] - \text{Tr}[\rho \log(e^{-H}/Z)]$.

**Gradient:** $\nabla_\theta O = -\text{Tr}[\rho \partial_\theta H] + \text{Tr}[(e^{-H}/Z)\partial_\theta H] - \lambda h_\theta \delta_{H_\theta \in H_Q}$

No Golden-Thompson approximation needed. The gradient is the difference between the expectation of $\partial_\theta H$ in the data state $\rho$ and in the model state $e^{-H}/Z$. This is the quantum analogue of the classical Boltzmann machine gradient.

**Convergence guarantee:** $S(\rho \| e^{-H}/Z) \geq \|\rho - e^{-H}/Z\|_1^2 / (2\ln 2)$, so driving the relative entropy to zero drives the model to the target.

### Gibbs state preparation

The paper discusses two methods for preparing $e^{-H}/Z$ (the bottleneck subroutine):

1. **Chowdhury-Somma (2016):** $O(\sqrt{N/Z} \cdot \text{polylog}(1/\varepsilon))$ cost, efficient when $Z = \Theta(N/\text{polylog}(N))$
2. **[[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Quantum Metropolis]]:** $O(\|H\|^2/(\varepsilon\sqrt{\delta}))$ cost, better when high precision isn't needed and Hamiltonian simulation is cheap

### Commutator training (Appendix D)

A third approach using Hadamard's lemma: expand the derivative of $e^{-H}$ as a commutator series $\partial_\theta H + [H, \partial_\theta H]/2! + [H,[H,\partial_\theta H]]/3! + \cdots$, truncated at low order. More accurate than Golden-Thompson when $\|[H, \partial_\theta H]\| \ll 1$ but less numerically stable.

---

## Key results

### Complexity

**Theorem 1.** Training for $N_\text{epoch}$ epochs with $M$ Hamiltonian terms, gradient accuracy $\varepsilon$, requires $O(N_\text{epoch} M^2/\varepsilon^2)$ queries to the Gibbs state oracle and/or training data oracle.

### Lower bounds

**Lemma 1.** Relative entropy training inherits the sample complexity lower bound from quantum state tomography: $\Omega(Dr/[\varepsilon^2 \log(D/r\varepsilon)])$ copies of the target state are needed for rank-$r$ states in $\mathbb{C}^{D \times D}$.

**Lemma 2.** POVM-based training can't find a state with overlap $\omega(1/\sqrt{N})$ with the training state using $o(\sqrt{N})$ queries (reduces to Grover search).

### Numerical results (small-scale exact simulations)

- Fermionic QBMs outperform classical Boltzmann machines on generative tasks for step-function data (5-8 visible units)
- Relative entropy training achieves graphical accuracy on 2-qubit tomography in ~5 epochs
- Pure states are harder to learn than mixed states (need $\|H\| \to \infty$, rough landscape)
- Mean-field approximations via the same framework converge in ~1 epoch

---

## Comparison with prior work

| Method | Learns quantum terms? | Approximation | Training data |
|---|---|---|---|
| Amin et al. (2016) | No (for computational basis data) | Golden-Thompson | Classical bitstrings |
| **POVM training (this paper)** | Yes (via non-diagonal POVMs) | Golden-Thompson | POVM measurement records |
| **Relative entropy training (this paper)** | Yes (exact gradients) | None | Copies of $\rho$ |
| **Commutator training (this paper)** | Yes | Truncated commutator series | POVM measurement records |

---

## Limits / caveats

- **Gibbs state preparation is the bottleneck.** All complexity bounds assume an oracle for $e^{-H}/Z$; the actual cost depends on which Gibbs sampler you use, and none are efficient in general. The paper acknowledges this honestly.
- **Scaling is limited by classical simulation.** All numerics are exact diagonalisation on $\leq 8$ qubits. Whether the advantage of fermionic QBMs over classical BMs persists at scale is unknown.
- **Overfitting.** The paper notes that quantum models with many parameters may overfit, especially with $H_{pqrs}$ terms providing a lot of freedom even without hidden units. This isn't resolved.
- **Pure states are hard.** Relative entropy training struggles with near-pure target states because $\|H\| \to \infty$ is required, making the loss landscape rough. Convergence is slow.
- **Golden-Thompson gap.** The lower bound from the Golden-Thompson inequality can be loose, leading to biased gradients. Commutator training can be more accurate but less stable.
- **Connection to trainability:** The [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|barren plateaus]] paper (by some of the same authors) later showed that variational quantum circuits can have vanishing gradients when entanglement grows. For QBMs, the gradient $\text{Tr}[\rho \partial_\theta H] - \text{Tr}[(e^{-H}/Z)\partial_\theta H]$ doesn't obviously suffer this (the signal comes from differences in expectations, not from randomly initialised unitaries), but trainability of deep QBMs remains an open question.

---

## Reusable ideas

1. [[Relative Entropy Gradient for Quantum Model Training]] — gradient of $S(\rho \| e^{-H}/Z)$ is a simple difference of expectations: $\text{Tr}[\rho \partial_\theta H] - \text{Tr}[(e^{-H}/Z)\partial_\theta H]$; no approximation, no time-ordering issues; the quantum analogue of the classical Boltzmann gradient
2. [[Golden-Thompson Bound for Quantum Objective Functions]] — use the Golden-Thompson inequality $\text{Tr}[e^{A+B}] \geq \text{Tr}[e^A e^B]$ to replace an intractable quantum objective with a computable lower bound; the bound is tight when $[A, B] = 0$, recovering the classical case

---

## References within this paper

- Amin et al. (2016), arXiv:1601.02036 — prior QBM training via Golden-Thompson with computational basis data; can't learn quantum terms
- Wiebe, Kapoor, Svore (2016) — quantum deep learning; polynomial speedups for Boltzmann training
- Hinton (2002) — contrastive divergence for classical Boltzmann machines
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Temme et al. (2011)]] — quantum Metropolis as Gibbs sampler
- Chowdhury & Somma (2016), arXiv:1603.02940 — alternative Gibbs state preparation via LCU
- Poulin & Wocjan (2009), arXiv:0905.2199 — quantum Gibbs sampling
- Haah, Harrow et al. (2015), arXiv:1508.01797 — sample-optimal tomography lower bounds

---

## Cross-links

### Paper notes
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]]

### Trick cards
- [[Relative Entropy Gradient for Quantum Model Training]]
- [[Golden-Thompson Bound for Quantum Objective Functions]]
- [[Coherent Encoding of Classical Distributions for Quantum Speedup]]
