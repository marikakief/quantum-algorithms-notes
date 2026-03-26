# Perturbative Error Correction for Truncated Hamiltonians

> **Source:** Motta, Ye, McClean, Li, Minnich, Babbush, Chan, arXiv:1808.02625
> **Tags:** #trick #error-correction #classical-postprocessing #quantum-chemistry #truncation #perturbation-theory

## What it does

Reduces the energy error from Hamiltonian truncation ($H \to H'$) by roughly an order of magnitude, allowing more aggressive truncation thresholds while staying within chemical accuracy. Uses only classically computable quantities — no additional quantum resources.

## The trick

Two corrections, both classical:

**1. Correlation energy referencing.** Instead of comparing total energies $E$ and $E'$, compare correlation energies $E_c = E - E_{\text{HF}}$ and $E'_c = E' - E'_{\text{HF}}$. Since both $E_{\text{HF}}$ and $E'_{\text{HF}}$ are classically computable (just Hartree-Fock on $H$ and $H'$ respectively), the truncation errors in the mean-field part cancel. This exploits the fact that in chemical systems, the correlation energy is a small fraction of the total energy, and truncation errors are dominated by two-electron terms whose mean-field contributions partially cancel.

**2. First-order perturbative correction.** Compute $\langle \psi | H - H' | \psi \rangle$ using a classically obtained wavefunction $\psi$ (e.g., CCSD). The corrected correlation energy $E'_c + \langle \psi | H - H' | \psi \rangle$ is accurate to $O(\varepsilon^2)$ if $\psi$ is a good approximation to the true ground state. This is standard first-order perturbation theory applied to the operator difference $H - H'$.

Combined effect: for H$_2$O at cc-pVDZ, a truncation threshold of $\varepsilon = 10^{-2}$ a.u. (quite aggressive) achieves sub-milliHartree accuracy in the correlation energy after correction.

## When to reach for it

- Any time you truncate a Hamiltonian for quantum simulation (Cholesky truncation, eigenvalue truncation, THC fitting error, active space projection)
- When the truncation changes the total energy substantially but you only care about relative energies or correlation energies
- As a classical postprocessing step to validate or improve quantum simulation results without additional quantum cost

## Complexity

- **Classical cost:** One Hartree-Fock calculation on $H'$ (scales as $O(N^3)$) plus one expectation value $\langle \psi | H - H' | \psi \rangle$ using a classical wavefunction (scales as $O(N^4)$ for CCSD, or cheaper for HF/MP2)
- **Error reduction:** From $O(\varepsilon)$ to $O(\varepsilon^2)$ in correlation energy

## Caveat

The correction quality depends on how good the classical wavefunction $\psi$ is. For strongly correlated systems where CCSD is poor (e.g., transition metal clusters at stretched geometries), the first-order correction may not achieve the full $O(\varepsilon^2)$ improvement. Also, this only corrects the operator truncation error — [[Trotterized Time Evolution for Chemistry|Trotter error]] from the time-evolution approximation is a separate issue.

## Related notes
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]]
