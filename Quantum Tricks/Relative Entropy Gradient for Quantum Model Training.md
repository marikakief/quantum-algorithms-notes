# Relative Entropy Gradient for Quantum Model Training

> **Source:** Mária Kieferová and Nathan Wiebe, arXiv:1612.05204  
> **Tags:** #trick #quantum-machine-learning #Boltzmann-machine #relative-entropy #Gibbs-state #tomography

## What it does

Provides exact, approximation-free gradients for training a quantum generative model by minimising the quantum relative entropy between a target state and a parameterised Gibbs state.

## The trick

Given a target density matrix $\rho$ and a parameterised Hamiltonian $H = \sum_j \theta_j H_j$, define the model state $\sigma = e^{-H}/Z$ and the objective $O = -S(\rho \| \sigma) = -\text{Tr}[\rho \log \rho] + \text{Tr}[\rho \log \sigma]$.

The gradient with respect to parameter $\theta_j$ is:

$$\frac{\partial O}{\partial \theta_j} = -\text{Tr}[\rho \, H_j] + \text{Tr}[\sigma \, H_j]$$

This is exact — no Golden-Thompson inequality, no Baker-Campbell-Hausdorff truncation, no time-ordering issues. The gradient is simply the difference between the expectation of $H_j$ in the data distribution and in the model distribution. When $\rho = \sigma$, the gradient vanishes (fixed point of training).

**Why it works:** the derivative $\partial_\theta \log(e^{-H}/Z)$ involves $\text{Tr}[e^{-H} \partial_\theta H]/\text{Tr}[e^{-H}]$, which is just the thermal expectation $\langle \partial_\theta H \rangle_\sigma$. The non-commutativity of $H$ and $\partial_\theta H$ doesn't matter because $\partial_\theta \text{Tr}[\rho \log(e^{-H}/Z)] = -\text{Tr}[\rho \partial_\theta H] + \partial_\theta \log Z = -\text{Tr}[\rho H_j] + \text{Tr}[\sigma H_j]$.

**Convergence:** $S(\rho \| \sigma) \geq \|\rho - \sigma\|_1^2 / (2\ln 2)$, so $S \to 0$ forces $\sigma \to \rho$ in trace distance.

## When to reach for it

- Training quantum Boltzmann machines for generative modelling or tomography
- Any setting where you want to fit a parameterised thermal state to data
- Quantum Hamiltonian learning: given access to thermal expectations of a physical system, infer the Hamiltonian
- Mean-field approximations: restrict $H$ to a product form and optimise

## Complexity

Each gradient step requires estimating $2M$ expectations ($M$ in $\rho$, $M$ in $\sigma$), each to precision $\varepsilon/M$ for overall gradient accuracy $\varepsilon$. Per epoch: $O(M^2/\varepsilon^2)$ queries to the Gibbs state oracle and training data oracle.

## Caveat

- Requires preparing the Gibbs state $e^{-H}/Z$ at each step — this is the computational bottleneck and is not efficient in general.
- $S(\rho \| \sigma)$ is not symmetric: $S(\rho \| \sigma) \neq S(\sigma \| \rho)$. The gradient drives $\sigma$ toward $\rho$, not the reverse.
- For near-pure $\rho$, $\|H\|$ must be large (the corresponding temperature is very low), making the loss landscape rough and convergence slow.
- The Hamiltonian model must be expressive enough to represent $\rho$. If key terms are missing, training converges to the closest approximation within the model class.

## Related notes

- [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Golden-Thompson Bound for Quantum Objective Functions]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]
