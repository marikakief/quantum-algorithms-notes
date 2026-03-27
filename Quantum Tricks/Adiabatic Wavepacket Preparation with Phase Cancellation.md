
> **Source:** Jordan, Lee, Preskill, Science 336, 1130 (2012); arXiv:1111.3633
> **Tags:** #trick #adiabatic #state-preparation #scattering #wavepacket #QFT

## What it does
Prepares non-eigenstate scattering states (wavepackets) of an interacting theory by adiabatically turning on the interaction, while preventing the wavepackets from propagating and scattering prematurely.

## The trick

The standard approach: start with wavepackets in the free theory, adiabatically increase the coupling $\lambda_0$ from 0 to its final value. The adiabatic theorem ensures each eigenstate evolves to the corresponding eigenstate of the interacting theory.

**The problem:** Wavepackets aren't eigenstates — they're superpositions of momentum modes. During adiabatic evolution, different modes acquire different dynamical phases, so the wavepacket propagates and broadens. If the particles collide before $\lambda_0$ reaches its target, you get unwanted scattering.

**The fix:** Divide the adiabatic path into $J$ steps. At each step $j$, sandwich the adiabatic evolution $U_j$ between backward and forward time evolutions:

$$M_j = e^{iH(\frac{j+1}{J})\frac{\tau}{2J}} \cdot U_j \cdot e^{iH(\frac{j}{J})\frac{\tau}{2J}}$$

The forward and backward evolutions approximately cancel the dynamical phases accumulated during $U_j$, while the adiabatic change of the eigenbasis is preserved. As $J \to \infty$, the dynamical phases converge to zero and the wavepacket stays localised throughout the preparation.

## When to reach for it

- Preparing scattering states in any quantum field theory simulation
- Any adiabatic state preparation where the target state is a *superposition* of eigenstates, not a single eigenstate
- When you need to adiabatically dress a state (e.g., turning on interactions) without it dispersing

## Complexity
Each sandwich step costs 3 [[Hamiltonian simulation]]s of duration $\tau/2J$. Total: $3J$ simulations, each of time $\sim \tau/J$. The dominant cost is the adiabatic slowness requirement ($\tau$ scales inversely with the energy gap squared), not the phase cancellation overhead.

## Caveat
The gap must remain open throughout the adiabatic path. Near a phase transition ($m \to 0$), the gap closes and $\tau$ diverges as $(\lambda_c - \lambda_0)^{-\nu}$. The $J \to \infty$ limit is an idealisation; finite $J$ gives residual phase errors that must be bounded separately.

## Related notes
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]]
- [[Adiabatic State Preparation for Chemistry]] — simpler version for eigenstate preparation
- [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]] — related idea of adapting the schedule to the local gap
