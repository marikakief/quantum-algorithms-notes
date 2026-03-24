# Trotterized Time Evolution for Chemistry

> **Source:** Trotter (1959), Proc. Am. Math. Soc. 10, 545; Aspuru-Guzik et al., Science 309, 1704 (2005); O'Malley, Babbush et al., arXiv:1512.06860 (2016)
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #Trotter #time-evolution #phase-estimation #quantum-chemistry #product-formula

## What it does

Approximates the time evolution operator $e^{-iHt}$ for a sum-of-terms Hamiltonian $H = \sum_\gamma g_\gamma H_\gamma$ by a product of individually-exponentiable terms. Enables quantum simulation of chemistry Hamiltonians without implementing the full matrix exponential.

## The trick

If $H = A + B$ and $[A, B] \neq 0$, then $e^{-i(A+B)t} \neq e^{-iAt} e^{-iBt}$. The first-order Trotter approximation is:

$$e^{-iHt} = e^{-it\sum_\gamma g_\gamma H_\gamma} \approx U_\text{Trot}(t) \equiv \left(\prod_\gamma e^{-ig_\gamma H_\gamma t/\rho}\right)^\rho$$

The error in this approximation is:

$$\|e^{-iHt} - U_\text{Trot}(t)\| = O\left(\frac{t^2}{\rho} \sum_{\gamma < \delta} |g_\gamma g_\delta| \|[H_\gamma, H_\delta]\|\right)$$

So error scales as $O(t^2/\rho)$. To achieve error $\epsilon$, use $\rho = O(t^2/\epsilon)$ Trotter steps.

**For molecular Hamiltonians** (H decomposed into Pauli strings via [[Bravyi-Kitaev Transformation]]):

$$U_\text{Trot}(t) = \left(e^{-ig_1 Z_0 t/\rho} \cdot e^{-ig_2 Z_1 t/\rho} \cdot e^{-ig_3 Z_0 Z_1 t/\rho} \cdot e^{-ig_4 X_0 X_1 t/\rho} \cdot e^{-ig_5 Y_0 Y_1 t/\rho}\right)^\rho$$

Each factor $e^{-ig_\gamma P_\gamma t}$ for a Pauli string $P_\gamma$ is straightforward to implement: diagonalize with basis rotations, apply a $R_z(2g_\gamma t)$ rotation, undo basis rotations.

**Trotter ordering matters.** The first-order error depends on the commutators of the ordered terms. For small systems, classically search over all orderings to find the one minimizing Trotter error at each time step. O'Malley et al. do this per-bond-length for H₂ (see Table I of the paper).

**Controlled Trotter evolution** (for phase estimation): The entire $U_\text{Trot}(t)$ can be conditioned on an ancilla qubit. Each $e^{-ig_\gamma P_\gamma t}$ becomes a controlled rotation, implementable with standard gate decompositions.

## When to reach for it

- Simulating time evolution under a molecular or lattice Hamiltonian on a quantum computer.
- As the core subroutine in [[Iterative Phase Estimation (Kitaev)]] for quantum chemistry.
- When the Hamiltonian is a sum of efficiently exponentiable terms (Pauli strings, lattice interactions, etc.).
- As a baseline simulation method before reaching for more advanced techniques (LCU, QSVT).

## Complexity

- **Trotter steps** to achieve precision $\epsilon$ at time $t$: $\rho = O(t^2 \Lambda^2 / \epsilon)$ for first-order, where $\Lambda = \sum_\gamma |g_\gamma|$ is the spectral norm of the perturbation.
- **Gate cost per Trotter step:** $O(N^4 \log N)$ for a molecular Hamiltonian with $N$ orbitals (BK encoding, $O(N^4)$ Pauli terms, $O(\log N)$ gates each).
- **Total gate count:** $O(N^4 \log N \cdot t^2/\epsilon)$ — expensive compared to LCU-based methods.
- **Controlled version:** $\approx 2\times$ overhead per Trotter step to make it controlled on an ancilla.

## Caveat

- First-order Trotter has poor constant factors. Second-order (Suzuki) halves the error without doubling the cost; higher-order formulas help asymptotically but blow up constants.
- For chemistry, the Trotter error depends on commutators of Pauli terms that have specific structure: diagonal terms (all $Z$) commute with each other, so the error comes only from non-diagonal terms. This means chemistry Hamiltonians often have smaller Trotter errors than generic Hamiltonians.
- O'Malley et al.'s PEA experiment used $\rho = 1$ Trotter step, achieving an error of $(1 \pm 1) \times 10^{-2}$ Hartree — well outside chemical accuracy. More steps would require longer coherent evolution than the device supports.
- LCU-based methods (Taylor series simulation, QSVT) have better asymptotic complexity than Trotter for chemistry, but Trotter remains simpler to implement and competitive for small systems.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Bravyi-Kitaev Transformation]]
- [[Variational Quantum Eigensolver (VQE)]]
