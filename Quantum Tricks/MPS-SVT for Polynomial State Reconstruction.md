# MPS-SVT for Polynomial State Reconstruction

> **Source:** Cramer, Plenio, Flammia, Somma, Gross, Bartlett, Landon-Cardinal, Poulin, Liu, arXiv:1101.4366
> **Tags:** #trick #MPS #tomography #SVT #compressed-sensing #DMRG #optimisation

## What it does
Adapts the singular value thresholding (SVT) algorithm for matrix completion to work in polynomial time by exploiting MPS structure — replacing the exponentially expensive diagonalisation step with DMRG.

## The trick

**The problem:** Given local measurement data $p_{m,i} = \text{tr}[\hat{\sigma}_i \hat{P}_m^i]$ from $k$-site local tomography, find a low-rank matrix (quantum state) consistent with these constraints.

**Standard SVT** (Candès-Recht, 2009): iteratively find the top eigenstate of a matrix $\hat{Y}_n$ and project towards consistency. Each iteration requires diagonalising a $2^N \times 2^N$ matrix — exponential cost.

**The MPS insight:** At every iteration, $\hat{Y}_n = \sum_{m,i} a_{m,i} \hat{P}_m^i$ where $\hat{P}_m^i$ are local Pauli products on $k$ sites. This is a local Hamiltonian! Finding its top eigenstate is a standard DMRG/MPS ground state problem, solvable in $O(\text{poly}(N, D^3))$ time.

**Algorithm:**
1. Set $\hat{R} = \sum_{m,i} p_{m,i} \hat{P}_m^i / 2^N$
2. Initialise $\hat{Y}_0 = 0$ (or $\hat{R}$)
3. At step $n$: find top eigenstate $|y_n\rangle$ of $\hat{Y}_n$ via DMRG
4. Compute $\hat{X}_n = y_n \sum_{m,i} \langle y_n|\hat{P}_m^i|y_n\rangle \hat{P}_m^i / 2^N$
5. Update $\hat{Y}_{n+1} = \hat{Y}_n + \delta_n(\hat{R} - \hat{X}_n)$
6. Repeat until convergence

**Performance:** For the critical Ising model, infidelity scales as $N^2/n$ (iterations). Converges for random Hamiltonians and noisy W-state data up to 20 qubits.

## When to reach for it

- Reconstructing quantum states from local measurement data when only $k$-body reduced density matrices are available
- Any convex optimisation over quantum states where the solution is expected to have MPS structure (low entanglement)
- Compressed sensing / matrix completion problems where the "matrix" is a quantum state and the measurements are local observables
- Post-processing data from 1D quantum simulators (ion traps, optical lattices) where global measurements are infeasible

## Complexity

- **Per iteration:** $O(\text{poly}(N, D^3))$ for DMRG eigenstate finding
- **Total iterations:** Empirically $O(N^2)$ for fixed accuracy
- **Overall:** Polynomial in $N$ for fixed bond dimension, but with no rigorous convergence proof for the MPS-modified version

## Caveat

- **No convergence proof.** Standard SVT has provable convergence. The MPS-modified version replaces the exact diagonalisation with DMRG (which finds the best MPS approximation, not necessarily the exact top eigenstate). Convergence is observed numerically but not guaranteed.
- **DMRG quality depends on bond dimension.** If the true top eigenstate of $\hat{Y}_n$ requires large bond dimension, DMRG may find a poor approximation, and the algorithm may stall.
- **Scaling with $k$.** The number of local observables grows as $O(N d^{2k})$, so large $k$ (needed for high bond dimension) increases measurement cost.

## Related notes
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]
- [[Parent Hamiltonian Witness for State Certification]]
- [[Sequential Disentanglement for MPS Tomography]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
