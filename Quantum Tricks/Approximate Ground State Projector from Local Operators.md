# Approximate Ground State Projector from Local Operators

> **Source:** Hastings, arXiv:0705.2024
> **Tags:** #trick #area-law #Lieb-Robinson #ground-state #spectral-gap

## What it does
Decomposes the ground state projector $P_0 = |\Psi_0\rangle\langle\Psi_0|$ of a gapped local Hamiltonian into a product of three operators $O_B O_L O_R$, each supported on local regions, with exponentially small error in the boundary width.

## The trick
Given a 1D Hamiltonian $H$ with spectral gap $\Delta E$ and [[Commutator-Sensitive Lieb-Robinson Bound|Lieb-Robinson velocity]] $v$, split $H = H_L + H_B + H_R$ by grouping terms to the left, boundary, and right of a cut at site $j$. Apply a Gaussian time filter:

$$\tilde{H}_L^0 = \frac{\Delta E}{\sqrt{2\pi q}} \int_{-\infty}^{\infty} dt \, H_L(t) \, e^{-(t\Delta E)^2/2q}$$

with $q = l\Delta E/(6v)$ and $l$ the boundary half-width. The Gaussian kills high-frequency (excited-state) contributions, while the Lieb-Robinson bound confines the resulting operator to a region near its original support. Define $O_L$ and $O_R$ as projectors onto low-eigenvalue subspaces of the localised versions $M_L$ and $M_R$, and $O_B$ as the localised version of the filtered projector $P_q$.

Then $\|O_B O_L O_R - P_0\| \leq C_1(\xi) \exp(-l/\xi')$ where $\xi' = 6\max(2v/\Delta E, \xi_C)$ and $C_1(\xi) = \text{poly}(\xi)$.

The operators have support: $O_L$ on $X_{1,j}$, $O_R$ on $X_{j+1,N}$, $O_B$ on $X_{j-l+1,j+l}$.

## When to reach for it
- Proving area laws: the local decomposition converts global ground-state properties into statements about boundary operators
- Any argument where you need to approximate a global projector by a product of local operators
- Constructing quasi-adiabatic continuation operators that respect locality

## Complexity
The approximation error decays exponentially: $\epsilon(l) \sim \exp(-l/\xi')$. To achieve error $\epsilon$, need boundary width $l \sim \xi' \ln(1/\epsilon)$. Each operator acts on $O(l)$ sites.

## Caveat
Requires a spectral gap. The construction fails for gapless systems. The polynomial prefactor $C_1(\xi)$ can be large, and the effective correlation length $\xi'$ scales as $v/\Delta E$, so for small gaps the boundary region must be wide. The original construction did not guarantee $O_B$ is Hermitian positive-definite, though a correction using $O_B^\dagger O_B$ fixes this at the cost of degrading the error from $\epsilon$ to $O(\sqrt{\epsilon})$.

## Related notes
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]]
- [[Entanglement Bootstrap from Approximate MPS]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
