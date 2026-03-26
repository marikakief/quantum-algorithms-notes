# ASCI Overlap Estimation for QPE Initial State

> **Source:** Tubman, Mejuto-Zaera, Epstein, Hait, Levine, Huggins, Jiang, McClean, Babbush, Head-Gordon, Whaley, arXiv:1809.05523 (2018)
> **Tags:** #trick #state-preparation #phase-estimation #classical-method #ASCI #overlap #initial-state

## What it does

Uses the classical Adaptive Sampling Configuration Interaction (ASCI) method to estimate whether a candidate initial state has adequate squared overlap $|\langle \psi_\text{in} | \Psi_0 \rangle|^2$ with the true ground state, without needing the ground state explicitly. ASCI converges the dominant determinant coefficients much faster than the total energy, making overlap estimates reliable at modest computational cost.

## The trick

**Core insight:** The coefficient of the dominant Slater determinant in the ground-state CI expansion — equivalently, the squared overlap of the HF state or any single-determinant reference with the true ground state — converges with the number of ASCI determinants far more rapidly than the total correlation energy.

Concretely: for CN radical, the dominant coefficient stabilises with $\sim 10^4$ ASCI determinants (overlap estimate $\sim$0.9 already reliable), while chemical accuracy in energy requires $> 10^6$ determinants. The convergence gap spans two to three orders of magnitude.

**ASCI ranking relation.** The algorithm builds an approximate ground state $|\Psi_\text{ASCI}\rangle = \sum_{i \in S} C_i |D_i\rangle$ by iteratively selecting the most important determinants. At each iteration, the importance of an as-yet-unselected determinant $|D_i\rangle$ is estimated via:

$$C_i^{(k+1)} \approx \frac{\sum_{j \in S} H_{ij} C_j^{(k)}}{E^{(k)} - H_{ii}}$$

This is a first-order perturbation-theory estimate for the CI coefficient (related to the CIPSI method). Determinants are ranked by $|C_i^{(k+1)}|$ and the top candidates are added. The iteration continues until desired accuracy.

**Overlap estimation procedure:**
1. Run ASCI to convergence of the dominant coefficient (not energy).
2. For a candidate state $|\psi_\text{in}\rangle$:
   - If single determinant $|D_\text{ref}\rangle$: overlap is $|C_{D_\text{ref}}|^2 = |\langle D_\text{ref} | \Psi_\text{ASCI}\rangle|^2$.
   - If multi-determinant $\sum_\ell \alpha_\ell |D_\ell\rangle$: overlap is $|\sum_\ell \alpha_\ell^* C_{D_\ell}|^2$.
3. Validate convergence: check that the estimate is stable as more ASCI determinants are added.
4. For strongly correlated systems (electron gas), extrapolate against the PT2 energy correction to remove finite-ASCI-size bias.

**Basis-choice optimisation.** Natural orbital rotations (diagonalise the ASCI 1-RDM, then re-express the wavefunction in the new basis) improve single-determinant overlap for molecules with stretched bonds. For lattice models, the plane-wave basis gives better HF overlap at small $U/t$; spatial orbitals are better at large $U/t$ near half-filling.

## When to reach for it

- Before running QPE resource estimates, to verify that state preparation is not the limiting cost.
- To determine whether a single HF determinant suffices (saves quantum circuit complexity), or whether a multi-determinant expansion is needed.
- To identify the number of determinants $L$ needed to achieve a target overlap (e.g. $|\langle \psi_\text{in} | \Psi_0 \rangle|^2 \geq 0.9$), setting the cost of [[Sequential Multi-Determinant State Preparation]].
- For DMFT or embedding calculations: check overlap for the impurity Hamiltonian in each self-consistency iteration to warn when single-determinant QPE input will fail.

## Complexity

**Classical:**
- ASCI with $M$ determinants: roughly $O(M \cdot N^2)$ per iteration (matrix-vector product in the determinant basis, where the Hamiltonian is sparse). In practice, ASCI on systems with up to $10^7$ determinants has been run on supercomputing clusters.
- Overlap evaluation: $O(L \cdot M)$ once ASCI coefficients are available.

**Quantum:** None — this is entirely a classical preprocessing step.

## Caveat

- **ASCI can converge to the wrong state.** For frustrated or symmetry-broken systems (half-filling Hubbard, DMFT below quarter-filling), the ASCI starting determinant strongly biases which phase is found. The antiferromagnetic vs. spin-density-wave comparison in the paper is a concrete warning: wrong starting point → wrong overlap estimate.
- **Circular reasoning (addressed but not fully eliminated).** The overlap estimate uses ASCI to certify itself. The authors argue this is not circular because overlap converges much earlier than energy; but for systems where the top ASCI determinants are not yet converged, the overlap estimate is unreliable.
- **PT2 extrapolation assumes linearity.** For strongly correlated systems ($r_s \geq 5$ for HEG), the overlap-vs-PT2-correction extrapolation is only approximately linear. Errors at the 0.01-level are possible.
- **Basis choice must be right.** Natural orbitals improve the single-determinant overlap for molecules; using the wrong basis can make it look like more determinants are needed than actually are.

## Related notes
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — introduced here
- [[Sequential Multi-Determinant State Preparation]] — uses the $L$ determinants and $\alpha_\ell$ coefficients that ASCI provides
- [[Hamming-Distance Ordering for Multi-Determinant Preparation]] — ASCI also provides the Hamming structure between top determinants
- [[Iterative Phase Estimation (Kitaev)]] — QPE is the algorithm for which state prep overlap matters
- [[Variational Quantum Eigensolver (VQE)]] — alternative to QPE when ground state preparation is hard; mentioned as likely insufficient for FeMoco
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the FeMoCo resource estimate whose state preparation assumption this trick helps verify
