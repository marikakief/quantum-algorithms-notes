> **Source:** Tobias J. Osborne and Michael A. Nielsen, *Entanglement in a simple quantum phase transition*, Physical Review A 66:032110, 2002
> **Links:** [arXiv:quant-ph/0202162](https://arxiv.org/abs/quant-ph/0202162) · [PRA](https://doi.org/10.1103/PhysRevA.66.032110)
> **Tags:** #entanglement #quantum-phase-transition #Ising-model #XY-model #concurrence #von-Neumann-entropy #condensed-matter

---

## The computational problem

Not a computational problem per se — this is a condensed matter / quantum information theory paper. The question is: what is the entanglement structure of the ground state of a quantum many-body system undergoing a quantum phase transition, and can entanglement measures detect and characterise the transition?

The specific system is the 1D anisotropic XY model (infinite lattice, thermodynamic limit), with the transverse-field Ising model ($\gamma = 1$) as the primary case study.

---

## What the paper does

Calculates the entanglement in the ground state and thermal state of the 1D XY model analytically, in the thermodynamic limit. Uses the Jordan-Wigner transform to diagonalise the Hamiltonian, constructs exact one- and two-site reduced density matrices, then computes the von Neumann entropy (single-site entanglement) and Wootters concurrence (pairwise entanglement) as functions of the coupling parameter $\lambda$ and temperature $T$.

The central finding: the single-site von Neumann entropy peaks at the quantum critical point $\lambda_c = 1$ of the transverse Ising model, and the next-nearest-neighbour concurrence is maximised at criticality. This establishes that quantum phase transitions are signalled by entanglement structure — the critical point is where the lattice is "maximally entangled" in the sense that entanglement becomes delocalised across all length scales.

This was one of the first papers (alongside the concurrent Osterloh-Amico-Falci-Fazio work) to make the entanglement-criticality connection quantitative in an exactly solvable model. It launched a large subfield connecting quantum information measures to condensed matter phase transitions.

---

## The model

The anisotropic XY Hamiltonian on an $N$-site 1D lattice:

$$H = -\sum_{j=0}^{N-1} \left[\frac{\lambda}{2}\left((1+\gamma)\sigma_j^x\sigma_{j+1}^x + (1-\gamma)\sigma_j^y\sigma_{j+1}^y\right) + \sigma_j^z\right]$$

where $\gamma$ is the anisotropy parameter and $\lambda$ is the inverse field strength. The transverse Ising model is $\gamma = 1$:

$$H_{\text{Ising}} = -\sum_j \left[\lambda \sigma_j^x\sigma_{j+1}^x + \sigma_j^z\right]$$

### Jordan-Wigner solution

The Jordan-Wigner transform maps spin operators to fermions:

$$c_i = \prod_{j=0}^{i-1}[-\sigma_j^z]\,\sigma_i^-$$

This renders $H$ quadratic in fermion operators, which is then diagonalised by a Bogoliubov transformation $\eta_q = \sum_i (g_{qi}c_i + h_{qi}c_i^\dagger)$ giving

$$H = 2\sum_q \omega_q \eta_q^\dagger \eta_q - \sum_q \omega_q$$

with single-particle spectrum:

$$\omega_q = \sqrt{(\gamma\lambda\sin\phi_q)^2 + (1 + \lambda\cos\phi_q)^2}, \quad \phi_q = 2\pi q/N$$

The gap $\Delta = \min_q \omega_q$ closes at $\lambda_c = 1$ for $\gamma > 0$, signalling the quantum phase transition.

---

## Key results

### 1. Single-site von Neumann entropy peaks at criticality

The reduced density matrix of a single site in the ground state is

$$\rho_1 = \frac{I + \langle\sigma^x\rangle\sigma^x + \langle\sigma^z\rangle\sigma^z}{2}$$

where the magnetisation $\langle\sigma^x\rangle = (1-\lambda^{-2})^{1/8}$ for $\lambda > 1$ (zero for $\lambda \leq 1$) and $\langle\sigma^z\rangle$ is an elliptic integral. The von Neumann entropy $S = -\text{tr}(\rho_1 \log \rho_1)$ is maximised at $\lambda_c = 1$.

**Interpretation:** At $\lambda = 0$ and $\lambda \to \infty$, the ground state approaches a product state ($S \to 0$). At the critical point, correlations exist on all length scales, so a single site is maximally entangled with the rest of the lattice.

The single-site entropy is *not* a universal quantity — it has different critical exponents on the two sides of the transition because the magnetisation $\langle\sigma^x\rangle$ (which grows as $\lambda^{1/8}$) turns on only for $\lambda > 1$.

### 2. Two-site concurrence structure

The two-site reduced density matrix is constructed from correlation functions $\langle\sigma_0^x\sigma_r^x\rangle$, $\langle\sigma_0^y\sigma_r^y\rangle$, $\langle\sigma_0^z\sigma_r^z\rangle$ (expressed as Toeplitz determinants via Jordan-Wigner). The Wootters concurrence:

$$C(\varrho) = \max[0, \lambda_1 - \lambda_2 - \lambda_3 - \lambda_4]$$

where $\lambda_i$ are eigenvalues of $R = \sqrt{\sqrt{\varrho}\,\tilde{\varrho}\,\sqrt{\varrho}}$.

Results for the transverse Ising model:
- **Nearest-neighbour concurrence:** peaks *near* $\lambda_c = 1$ but not exactly at it (maximum is slightly below $\lambda_c$)
- **Next-nearest-neighbour concurrence:** peaks exactly at the critical point, with value $C = 0.0044$
- **$r \geq 3$:** zero concurrence for all $\lambda$

At the critical point $\lambda_c = 1$, the exact values are $C_1 = 0.1946$ (nearest neighbour) and $C_2 = 0.0044$ (next-nearest neighbour).

### 3. Entanglement sharing explains the shifted peak

The nearest-neighbour concurrence peaks *below* the critical point because of entanglement monogamy: at criticality, each site must share entanglement with all other sites (correlations on all length scales), so the pairwise entanglement with any single neighbour is reduced compared to the pre-critical regime where entanglement is more localised. This is the [[Entanglement Redistribution at Quantum Phase Transitions]] mechanism.

### 4. Thermal entanglement

Entanglement persists at nonzero temperature in a region corresponding to Sachdev's quantum critical regime. Two notable features:
- For certain $\lambda$ values ($\lambda \approx 1.4$), nearest-neighbour entanglement *increases* with temperature — not an artifact of finite-size truncation, confirmed here in the thermodynamic limit
- Entanglement persists above the energy gap $\Delta$, suggesting quantum effects survive beyond the naive classical threshold $k_BT > \Delta$

---

## Comparison with concurrent work

| | Osborne-Nielsen (this paper) | Osterloh-Amico-Falci-Fazio (2002) |
|---|---|---|
| **Model** | XY model (full $\gamma$-$\lambda$ phase diagram) | XY model (same) |
| **System size** | Thermodynamic limit (analytic) | Finite chains + thermodynamic limit |
| **Entanglement measures** | von Neumann entropy + concurrence | Concurrence + derivative analysis |
| **Key finding** | Single-site entropy peaks at $\lambda_c$; entanglement sharing explains shifted concurrence peak | Concurrence derivative diverges at $\lambda_c$ with scaling related to universality class |
| **Temperature** | Full thermal analysis | Ground state only |

The two papers appeared simultaneously (both arXiv Feb 2002) and are complementary. Osterloh et al. focused more on universality and scaling of concurrence derivatives; Osborne-Nielsen emphasised the physical picture of entanglement redistribution and thermal persistence.

---

## Limits / caveats

- Only two-qubit entanglement measures are available (concurrence). Multi-party entanglement in the ground state — which is arguably the more physically relevant quantity — is not captured. The authors explicitly note that the AKLT model has constant single-site entropy across all parameter values despite a phase transition, so single-site entropy is not always diagnostic.
- The analysis is restricted to 1D. Higher-dimensional systems may have qualitatively different entanglement structure at criticality.
- The von Neumann entropy is *not* universal — different critical exponents on the two sides. Later work by Calabrese and Cardy (2004) showed that the *block* entanglement entropy (subsystem of $\ell$ sites, not just one site) is universal, scaling as $\frac{c}{3}\log\ell$ with the central charge $c$ of the CFT. This resolved the non-universality issue raised here.
- Concurrence is zero for $r \geq 3$, which doesn't mean there's no quantum correlation at longer range — just that pairwise entanglement as measured by concurrence can't detect it.

---

## Reusable ideas

1. [[Entanglement Redistribution at Quantum Phase Transitions]] — At criticality, entanglement delocalises from nearest-neighbour pairs to all length scales, reducing pairwise entanglement while increasing single-site entropy. Entanglement monogamy constrains the redistribution.

2. [[Jordan-Wigner Reduced Density Matrix Construction]] — Use the Jordan-Wigner transform to obtain exact correlation functions, then reconstruct reduced density matrices via the Pauli operator expansion. The symmetries of the Hamiltonian (reality, global phase flip) reduce the number of independent correlators.

---

## References within this paper

- Sachdev (1999), *Quantum phase transitions* — the standard reference for QPT theory and the quantum critical regime; framework for the entanglement-criticality connection
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden (2003)]] — later formalised the entanglement requirement for quantum speedup; this paper's physical picture of entanglement at criticality complements that computational perspective
- Osterloh-Amico-Falci-Fazio (2002), quant-ph/0202029 — concurrent independent work on the same model; complementary focus on universality of concurrence scaling
- Wootters (1998), PRL 80:2245 — the concurrence formula used throughout
- Pfeuty (1970), Ann. Physics 57:79 — exact solution of the transverse Ising model; provides the critical-point correlation functions
- Barouch-McCoy (1970, 1971), PRA 2:1075 and PRA 3:786 — correlation functions for the XY model
- Coffman-Kundu-Wootters (2000), PRA 61:052306 — entanglement monogamy bounds (CKW inequality), used to argue for entanglement redistribution at criticality
- O'Connor-Wootters (2001), PRA 63:052302 — maximal nearest-neighbour concurrence in translationally invariant rings; comparison benchmark

---

## Cross-links

### Paper notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — Vidal showed bounded entanglement implies classical simulability; this paper shows criticality maximises entanglement, connecting QPTs to the boundary of classical simulability
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]] — entanglement as a resource for quantum speedup; this paper provides the condensed matter perspective
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]] — Osborne's later work on quantum thermal sampling, building on the thermal entanglement picture
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]] — computational complexity of ground state problems in spin models including those studied here

### Trick cards
- [[Entanglement Redistribution at Quantum Phase Transitions]]
- [[Jordan-Wigner Reduced Density Matrix Construction]]
