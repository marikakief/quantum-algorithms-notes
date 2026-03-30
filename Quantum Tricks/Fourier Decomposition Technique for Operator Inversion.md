# Fourier Decomposition Technique for Operator Inversion

> **Source:** Childs, Kothari, Somma, SIAM J. Comput. 46(6):1920–1950, 2017; applied in CV setting by Arrazola, Kalajdzievski, Weedbrook, Lloyd, arXiv:1809.02622
> **Tags:** #trick #matrix-inversion #Fourier-integral #linear-systems #Hamiltonian-simulation

## What it does
Implements the inverse $A^{-1}$ of a Hermitian operator using only Hamiltonian simulation of $A$, ancilla state preparation, and postselective measurement. No eigenvalue decomposition or phase estimation required.

## The trick

Start from the identity for the odd function $g(x) = xe^{-x^2/2}$:

$$a^{-1} = \frac{i}{\sqrt{2\pi}} \int_{-\infty}^{\infty} dx\, \Theta(x) \int_{-\infty}^{\infty} dy\, ye^{-y^2/2} e^{-iaxy}$$

where $\Theta(x)$ is the Heaviside step function. This lifts to the operator level: $\hat{A}^{-1}$ and $e^{-i\hat{A}xy}$ share the same eigenbasis when $\hat{A}$ is Hermitian, so

$$\hat{A}^{-1} = \frac{i}{\sqrt{2\pi}} \int dx\, dy\, \Theta(x)\, ye^{-y^2/2} e^{-i\hat{A}xy}.$$

To implement this:
1. Prepare two ancilla registers in states encoding $\Theta(x)$ (step function) and $ye^{-y^2/2}$ (single-photon wavefunction)
2. Apply the controlled evolution $e^{-i\hat{A}\hat{X}\hat{Y}}$ where $\hat{X}$, $\hat{Y}$ are position operators of the ancillae
3. Measure ancillae in the momentum basis, postselect on $p = 0$

The postselected state of the target register is $\hat{A}^{-1}|f\rangle$.

In the discrete (qubit) setting, Childs-Kothari-Somma discretise the integrals and implement the controlled evolutions via [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]]. In the CV setting, the integrals are performed naturally by the continuous-variable modes.

## When to reach for it

- When you want to invert a Hermitian operator and have access to Hamiltonian simulation of that operator
- As an alternative to [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]]-based inversion when you want to avoid the QPE circuit overhead
- When the operator $A$ is naturally a differential operator and you're working in a CV architecture

## Complexity

The postselection probability is $O(\Delta^2)$ where $\Delta$ is the momentum measurement precision. The Hamiltonian simulation cost depends on $\|A\|$ and the evolution time. Total query complexity matches Chebyshev-based LCU approaches from [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|CKS (2015)]]: $O(\kappa \cdot \text{polylog}(1/\varepsilon))$ for condition number $\kappa$.

## Caveat

- Postselection probability decreases with precision, creating a tradeoff. [[Amplitude Amplification and Estimation|Amplitude amplification]] can partially offset this.
- The step function approximation introduces errors for small eigenvalues $|a| \lesssim 1/L$, effectively setting a condition number cutoff.
- In the CV implementation, preparing the step function state is non-trivial and requires heuristic optimisation (quantum neural networks).

## Related notes
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
