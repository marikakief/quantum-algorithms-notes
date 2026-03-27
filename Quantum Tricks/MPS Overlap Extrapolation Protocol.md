
> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748 (2024)
> **Tags:** #trick #state-preparation #MPS #DMRG #overlap #extrapolation #FeMoCo

## What it does

Estimates the squared overlap $|\langle\Phi(M)|\Phi(\infty)\rangle|^2$ between a finite bond dimension MPS (from DMRG) and the exact ground state, using only data from finite bond dimension calculations. No access to the true ground state is needed.

## The trick

Two empirically observed linear relations (in the systems tested):

**Relation 1 (infidelity vs. bond dimension):**

$$\log(1 - |\langle\Phi(M')|\Phi(\infty)\rangle|^2) \quad \text{is linear in} \quad (\log M')^2$$

**Relation 2 (overlap error vs. reference bond dimension):**

$$\log(|\langle\Phi(M')|\Phi(M'')\rangle|^2 - |\langle\Phi(M')|\Phi(\infty)\rangle|^2) \quad \text{is linear in} \quad (\log M'')^2 \quad (M' \ll M'')$$

**Protocol:**
1. Compute DMRG wavefunctions at bond dimensions $M_1' < M_2' < \ldots$ (small) and $M_1'' < M_2'' < \ldots$ (large, from reverse sweep).
2. For each small $M'$, compute overlaps $|\langle\Phi(M')|\Phi(M'')\rangle|^2$ for several large $M''$ values.
3. Use Relation 2 to extrapolate $|\langle\Phi(M')|\Phi(\infty)\rangle|^2$ for each small $M'$.
4. Use Relation 1 to extrapolate $|\langle\Phi(M_{\text{target}})|\Phi(\infty)\rangle|^2$ for the target bond dimension.

For FeMoCo: uses $M' = 1000, 1500$ and $M'' = 3000$–$6000$ (depending on MPS candidate), producing overlap estimates of $0.95$–$0.99$ for the three candidate MPS states.

## When to reach for it

- Before running QPE, to estimate how many repetitions will be needed (the cost scales as $\sim 1/p$ for sampling).
- For any quantum chemistry resource estimation that needs to account for state preparation overhead — this converts the abstract "assume overlap $p$" into a concrete estimate.
- To compare classical DMRG fidelity with QPE cost: if the extrapolated overlap at a given bond dimension is high enough that only 2–3 QPE samples are needed, the quantum overhead from imperfect state preparation is small.
- For benchmarking MPS quality in strongly correlated systems where exact diagonalisation is infeasible.

## Complexity

The classical cost is modest: computing DMRG at a few bond dimensions ($M' \sim 10^2$–$10^3$ and $M'' \sim 10^3$–$10^4$), plus overlaps between the resulting MPS states. The bottleneck is the largest-$M''$ DMRG calculation, which scales as $O(M''^3 N)$ per sweep for $N$ orbitals.

## Caveat

**This is entirely empirical.** The two linear relations have no rigorous theoretical justification. They are validated on Fe$_2$S$_2$ and Fe$_4$S$_4$ (where exact ground states can be computed), but there is no guarantee they hold for larger or qualitatively different systems.

For FeMoCo specifically: at finite bond dimension, the energy ordering of competing spin-coupled states may differ from the true ordering. The MPS labelled "lowest energy" at $\chi = 2000$ may not correspond to the actual ground state. The paper handles this by preparing multiple candidates and using QPE to resolve the energies — but this multiplies the total cost.

The protocol also cannot distinguish between the prepared MPS having high overlap with the ground state vs. a low-lying excited state. If the MPS converges to an excited state, the extrapolated overlap is with that excited state, not the ground state.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[ASCI Overlap Estimation for QPE Initial State]]
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the original FeMoCo benchmark that deferred state preparation; this trick enables concrete overlap estimates for FeMoCo
