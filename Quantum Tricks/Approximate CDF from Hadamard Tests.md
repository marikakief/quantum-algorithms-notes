# Approximate CDF from Hadamard Tests

> **Source:** Lin Lin and Yu Tong, arXiv:2102.11340 (PRX Quantum 3, 010318, 2022)
> **Tags:** #trick #spectral-measure #CDF #binary-search #Hadamard-test

## What it does

Constructs a smooth approximation to the spectral cumulative distribution function (CDF) of a Hamiltonian from Hadamard-test measurements, then inverts it via classical binary search to find eigenvalues.

## The trick

Given a Hamiltonian $H$ with spectral decomposition $H = \sum_k \lambda_k \Pi_k$ and a state $\rho$:

1. **Define the spectral CDF:** $C(x) = \sum_k p_k \cdot \mathbb{1}[\tau\lambda_k \leq x]$ where $p_k = \operatorname{Tr}[\rho\,\Pi_k]$.

2. **Approximate the step function** with a degree-$d$ trigonometric polynomial $F(x) \approx H(x)$ (the periodic Heaviside), accurate to $\varepsilon_F$ except in a transition region of width $\delta$ around the jump.

3. **Define ACDF:** $\tilde{C}(x) = (F * p)(x) = \sum_{|j| \leq d} \hat{F}_j e^{ijx} \operatorname{Tr}[\rho\, e^{-ij\tau H}]$.

4. **Estimate** $\tilde{C}(x)$ at chosen points via [[Importance Sampling for Fourier CDF Estimation|importance sampling]].

5. **Invert via binary search:** The ground-state energy is where $C(x)$ jumps from $0$ to $\geq p_0$. Binary search on $x$ with a CERTIFY subroutine (majority-voted ACDF evaluation) finds this jump to precision $\delta/\tau$.

The separation between quantum and classical is clean: the quantum computer runs $M$ simple Hadamard-test circuits (each with one ancilla qubit and controlled $e^{-ij\tau H}$), producing $M$ classical samples. All the intelligence — CDF construction, binary search, majority voting — is classical post-processing.

## When to reach for it

- Ground-state energy estimation when circuit depth must stay short (early fault-tolerant regime).
- When you want spectral information beyond just the lowest eigenvalue — the ACDF gives you the full distribution.
- As a lightweight alternative to QPE when you have limited ancilla qubits.

## Complexity

For ground-state energy to precision $\varepsilon$ with overlap $\eta$:
- Quantum: $M = \tilde{O}(\eta^{-2}\text{polylog}(\varepsilon^{-1}))$ measurements, $T_{\max} = \tilde{O}(\varepsilon^{-1})$
- Classical: $\tilde{O}(\eta^{-2}\text{polylog}(\varepsilon^{-1}))$ post-processing

## Caveat

- The ACDF is "blurred" by the transition width $\delta$: eigenvalues closer than $\delta$ are not resolved. Setting $\delta = \varepsilon\tau$ ensures the energy estimate is accurate to $\varepsilon$ but means you can't resolve spectral features finer than that.
- Requires a lower bound $\eta$ on the ground-state overlap. If unknown, you need a preliminary estimation step.
- The Hadamard test requires controlled $e^{-ij\tau H}$, which adds Hamiltonian simulation overhead.

## Related notes
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[Importance Sampling for Fourier CDF Estimation]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
