
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

So error scales schematically as $O(t^2/\rho)$, with the hidden constant set by coefficient-weighted commutator sums. To achieve error $\epsilon$, the step count must scale with that commutator/error scale, not just with $t^2/\epsilon$.

**Small illustrative molecular Hamiltonian** (the H$_2$-style five-Pauli-term form used in early experiments after symmetry reduction and fermion-to-qubit mapping):

$$U_\text{Trot}(t) = \left(e^{-ig_1 Z_0 t/\rho} \cdot e^{-ig_2 Z_1 t/\rho} \cdot e^{-ig_3 Z_0 Z_1 t/\rho} \cdot e^{-ig_4 X_0 X_1 t/\rho} \cdot e^{-ig_5 Y_0 Y_1 t/\rho}\right)^\rho$$

Each factor $e^{-ig_\gamma P_\gamma t}$ for a Pauli string $P_\gamma$ is straightforward to implement: diagonalize with basis rotations, apply a $R_z(2g_\gamma t)$ rotation, undo basis rotations.

**Trotter ordering matters.** The first-order error depends on the commutators of the ordered terms. For very small systems, one can classically search over all orderings to find the one minimizing Trotter error at each time step. O'Malley et al. do this per-bond-length for H₂ (see Table I of the paper). This is not feasible for generic chemistry with many terms.

**Controlled Trotter evolution** (for phase estimation): The entire $U_\text{Trot}(t)$ can be conditioned on an ancilla qubit. Each $e^{-ig_\gamma P_\gamma t}$ becomes a controlled Pauli rotation. The overhead depends on the native gate set, whether rotations are classically or coherently controlled, and the surrounding phase-estimation construction.

## When to reach for it

- Simulating time evolution under a molecular or lattice Hamiltonian on a quantum computer.
- As the core subroutine in [[Iterative Phase Estimation (Kitaev)]] for quantum chemistry.
- When the Hamiltonian is a sum of efficiently exponentiable terms (Pauli strings, lattice interactions, etc.).
- As a baseline simulation method before reaching for more advanced techniques (LCU, QSVT).

## Complexity

- **Terms per step:** up to $O(N^4)$ Pauli strings for a generic second-quantized molecular Hamiltonian before exploiting symmetry, sparsity, low-rank structure, or basis-specific simplifications.
- **Gate cost per term:** depends on encoding and connectivity. Bravyi-Kitaev-style Pauli strings can have $O(\log N)$ weight, while Jordan-Wigner strings can have $O(N)$ weight unless compiled with routing/swap-network structure.
- **Number of Trotter steps:** for first order, governed by coefficient-weighted commutator sums such as $\sum_{\gamma<\delta}|g_\gamma g_\delta|\|[H_\gamma,H_\delta]\|$. A crude bound can be written in terms of $\Lambda=\sum_\gamma |g_\gamma|$, but $\Lambda$ is a coefficient 1-norm/LCU-style normalization, not the spectral norm of a perturbation.
- **Total cost:** multiply the per-step term/gate cost by the required step count $\rho$.
- **Controlled version:** overhead is circuit- and gate-set-dependent; it is not universally a factor of 2.

## Caveat

- Fixed first-order Trotter has poor precision scaling. Second-order and higher-order Suzuki formulas change the timestep error order, often with favorable constants but with formula- and ordering-dependent costs.
- For chemistry, the Trotter error depends on nested commutators and on state-dependent energy bias. Diagonal terms (all $Z$) commute with each other, so important parts of the error come from non-diagonal structure. This means chemistry Hamiltonians often have smaller practical Trotter errors than generic norm bounds predict.
- O'Malley et al.'s PEA experiment used $\rho = 1$ Trotter step, achieving an error of $(1 \pm 1) \times 10^{-2}$ Hartree — well outside chemical accuracy. More steps would require longer coherent evolution than the device supports.
- LCU/QSP/qubitization methods have better asymptotic precision dependence under their access models, but Trotter remains simpler to implement and can be competitive for small systems or resource regimes where direct Pauli rotations are cheap.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Bravyi-Kitaev Transformation]]
- [[Variational Quantum Eigensolver (VQE)]]
