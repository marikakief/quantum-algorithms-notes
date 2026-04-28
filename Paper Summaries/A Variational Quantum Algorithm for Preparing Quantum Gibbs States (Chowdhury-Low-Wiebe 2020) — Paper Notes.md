> **Source:** Anirban N. Chowdhury, Guang Hao Low, Nathan Wiebe, *A Variational Quantum Algorithm for Preparing Quantum Gibbs States*, arXiv:2002.00055 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/2002.00055)
> **Tags:** #gibbs-state #variational #VQA #thermal-state #free-energy #entropy-estimation #NISQ

---

## The computational problem

Prepare a Gibbs state

$$
\rho_\beta = \frac{e^{-\beta H}}{\operatorname{Tr}(e^{-\beta H})}
$$

for a Hamiltonian $H$ on near-term hardware, without the large ancilla overheads and long coherent runtimes of [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Quantum Metropolis]], [[Coherent Metropolis Accept-Reject via Phase Estimation|phase-estimation based thermalisation]], or exact [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]] / [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] style methods.

The optimisation objective is the Helmholtz free energy

$$
F_\beta(\rho) = \operatorname{Tr}(H\rho) - \beta^{-1} S(\rho), \qquad S(\rho) = -\operatorname{Tr}(\rho \log \rho),
$$

whose minimiser is exactly $\rho_\beta$. The difficulty is that the entropy term is nonlinear and not directly observable.

## What the paper does

The paper gives a variational Gibbs-state preparation algorithm that estimates the free energy of an ansatz state and then minimises it classically. The key move is to approximate $\log \rho$ by a truncated Fourier series, turning entropy estimation into a weighted sum of simpler expectation values that can be measured on a shallow circuit.

Conceptually, this is a NISQ-friendly thermal-state version of [[Variational Quantum Eigensolver (VQE)|VQE]]: replace energy minimisation by free-energy minimisation, and pay for it with an entropy-estimation subroutine.

## The algorithm / construction

### Ansatz and purification picture

The algorithm prepares a mixed trial state $\rho(\theta)$, typically by purifying it on a larger Hilbert space and tracing out an environment register. Write

$$
\rho(\theta) = \operatorname{Tr}_E \left[ U(\theta) |0\rangle\langle 0| U(\theta)^\dagger \right].
$$

The variational objective is

$$
F_\beta(\theta) = \operatorname{Tr}[H\rho(\theta)] - \beta^{-1} S(\rho(\theta)).
$$

The energy term is standard: decompose $H = \sum_j h_j P_j$ into measurable observables and estimate $\operatorname{Tr}(H\rho)$ term by term.

### Fourier approximation to the logarithm

The entropy term is rewritten as

$$
S(\rho) = -\operatorname{Tr}(\rho \log \rho).
$$

The paper approximates the logarithm on the relevant spectral interval by a truncated Fourier series,

$$
\log x \approx a_0 + \sum_{m=1}^{M} \big(a_m \cos(m x) + b_m \sin(m x)\big),
$$

or equivalently by a trigonometric polynomial after rescaling the spectrum. Plugging this into the entropy gives an estimator of the form

$$
S(\rho) \approx c_0 + \sum_{m=1}^{M} c_m \, \operatorname{Tr}[\rho f_m(\rho)],
$$

where each $f_m$ is simpler to access than $\log \rho$ itself. Operationally, the algorithm estimates these terms using circuits related to powers or controlled functions of the ansatz state, then combines them classically.

The important structural idea is not the exact coefficient formula, but the reduction:

1. approximate a hard matrix function $\log$ by a short trigonometric series,
2. measure each basis component separately,
3. reconstruct the entropy and hence the free energy in classical post-processing.

### Free-energy optimisation loop

For each parameter setting $\theta$:

1. Prepare the ansatz state $\rho(\theta)$.
2. Estimate the energy $E(\theta) = \operatorname{Tr}(H\rho(\theta))$.
3. Estimate the entropy $S(\rho(\theta))$ using the Fourier-series surrogate.
4. Form
   $$
   \widehat F_\beta(\theta) = \widehat E(\theta) - \beta^{-1} \widehat S(\rho(\theta)).
   $$
5. Update $\theta$ with a classical optimiser.

The paper studies a concrete ansatz based on [[Adiabatic State Preparation for Chemistry|adiabatic state preparation]] implemented in Trotterized form. In effect, the circuit family tries to stay near the thermal manifold of a slowly deformed Hamiltonian path.

### Why the high-temperature regime is easier

At high temperature, the Gibbs state is close to maximally mixed,

$$
\rho_\beta = \frac{I}{d} - \beta \frac{H - \operatorname{Tr}(H)I/d}{d} + O(\beta^2),
$$

so both the entropy landscape and the target state are better conditioned. The paper's efficiency statement is correspondingly local: if the initial variational parameters are already close to a global optimum, then preparing a constant-accuracy high-temperature Gibbs state is efficient.

## Key results

### Variational principle

The Gibbs state is the unique minimiser of the free energy:

$$
F_\beta(\rho) = \operatorname{Tr}(H\rho) - \beta^{-1}S(\rho).
$$

Thus, if the ansatz family contains or approximates $\rho_\beta$, variational optimisation can in principle recover it.

### Entropy-estimation reduction

The paper shows that the entropy can be approximated by a truncated Fourier expansion of the logarithm, so estimating $F_\beta(\rho)$ reduces to a finite number of simpler measurements plus classical post-processing. This is the main algorithmic contribution.

### Efficiency claim

For sufficiently high temperature and an initial parameter choice close enough to a global optimum, the method prepares a constant-error approximation to the Gibbs state efficiently. This is not a worst-case global convergence theorem, but a local tractability statement in the benign regime where NISQ heuristics might plausibly work.

### Numerical demonstration

The paper numerically studies 5-qubit examples using a Trotterized adiabatic ansatz and finds that the free-energy objective can guide the circuit toward reasonable thermal states, at least at small scale.

## Comparison with prior work

| Approach | Model | Strength | Weakness |
|---|---|---|---|
| [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Quantum Metropolis]] | fault-tolerant / long-coherence | principled thermalisation | heavy coherence and ancilla cost |
| [[Coherent Metropolis Accept-Reject via Phase Estimation|PE-based Gibbs preparation]] | fault-tolerant | cleaner complexity guarantees | not NISQ-friendly |
| Variational thermal algorithms | NISQ | shallow circuits, hardware-adaptable | heuristic optimisation, no global guarantee |
| **This paper** | NISQ variational | direct free-energy objective with explicit entropy estimator | high-temperature / good-initialisation regime, measurement overhead |

Relative to ordinary [[Variational Quantum Eigensolver (VQE)|VQE]], this paper solves a harder objective because the entropy term depends nonlinearly on the whole spectrum of $\rho$, not just an energy expectation. The Fourier-series trick is what makes the thermal extension plausible.

## Limits / caveats

- The convergence guarantee is local, not global. If the optimiser starts far from the right basin, nothing forces success.
- The clean efficiency claim is for high-temperature Gibbs states. Low-temperature thermal states are much harder because the entropy is smaller and the target is closer to a projector onto low-energy space.
- Entropy estimation is measurement-heavy compared with plain energy minimisation.
- The practical quality depends strongly on the ansatz family. A poor purification ansatz can make the free-energy landscape misleading.
- This is a heuristic NISQ algorithm, not a replacement for asymptotically sharp fault-tolerant Gibbs-state preparation.

## Reusable ideas

1. [[Fourier-Series Entropy Estimation for Variational Gibbs States]]: approximate $\log \rho$ by a short trigonometric series so entropy becomes a sum of measurable surrogate terms.
2. [[Free-Energy Variational Principle for Thermal State Preparation]]: use Helmholtz free energy, not energy alone, as the variational objective for mixed-state preparation.
3. [[Trotterized Adiabatic Ansatz for Thermal Variational States]]: recycle adiabatic-style circuit structure as a variational family for mixed-state targets.

## Cross-links

### Paper notes

- [[Variational Quantum Eigensolver (VQE)]]
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]

### Trick cards

- [[Fourier-Series Entropy Estimation for Variational Gibbs States]]
- [[Free-Energy Variational Principle for Thermal State Preparation]]
- [[Trotterized Adiabatic Ansatz for Thermal Variational States]]
