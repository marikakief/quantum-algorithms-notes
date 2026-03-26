# Stabilizer Subspace Projection for Error Mitigation

> **Source:** McClean, Jiang, Rubin, Babbush, Neven, arXiv:1903.05786
> **Tags:** #trick #error-mitigation #stabilizer-codes #post-processing #NISQ #quantum-error-correction

## What it does

Removes non-code-space components of a noisy quantum state using only Pauli measurements and classical post-processing — no ancilla qubits, no syndrome measurement, no fast feedback.

## The trick

For a stabilizer code with generators $\{S_1, \ldots, S_m\}$, the code-space projector is:

$$P = \prod_{i=1}^{m} \frac{I + S_i}{2} = \frac{1}{2^m} \sum_{M \in \mathcal{S}} M$$

where $\mathcal{S}$ is the stabilizer group. The corrected expectation value of a logical observable $\Gamma = \sum_j \gamma_j \Gamma_j$ is:

$$\langle \Gamma \rangle_\text{corr} = \frac{\text{Tr}(P\rho P\, \Gamma)}{\text{Tr}(P\rho P)}$$

Since logical operators commute with stabilizer elements, this reduces to:

$$\langle \Gamma \rangle_\text{corr} = \frac{1}{c \cdot 2^m} \sum_{j,k} \gamma_j \,\text{Tr}(\rho\, \Gamma_j M_k)$$

Every term $\text{Tr}(\rho\, \Gamma_j M_k)$ is a Pauli expectation value. Measure it by preparing $\rho$ and measuring in the appropriate basis. The state is destroyed each shot — that's fine because this is batch post-processing, not active correction.

Use [[Stochastic Stabilizer Sampling for Post-Processed QEC|stochastic sampling]] to avoid enumerating all $2^m$ group elements.

**With recovery:** For a correctable error $E_\alpha$ with known syndrome, project into the error subspace $(I + (-1)^{s_j} S_j)/2$ and apply recovery $R_\alpha$. This increases signal (more of the state contributes) at the cost of potentially introducing logical errors for high-weight uncorrectable errors.

**QSE relaxation:** Replace the fixed projector with an optimized linear combination $P_c = \sum_i c_i M_i$, where coefficients are found by solving a generalized eigenvalue problem. This allows using fewer check operators while still approximating the full projection, and can correct some *logical* errors when combined with a problem Hamiltonian that breaks the code-space degeneracy.

## When to reach for it

- You have a quantum code but can't do fast syndrome measurement or feedback (i.e., any current NISQ device).
- You want to study a code's performance under real device noise without implementing full fault-tolerant infrastructure.
- The code has non-local stabilizers that would require complicated ancilla circuits for standard syndrome extraction — here, non-locality is free.
- You're running a variational algorithm inside a code and want to improve result quality via post-processing.

## Complexity

- **Additional qubits:** Zero.
- **Additional circuit depth:** Zero. All correction is in classical post-processing.
- **Measurement overhead:** Depends on the state quality, not the code size. For a state with fraction $w$ outside the code space, the variance of the corrected estimator scales as $\sim w(2-w)/4$. At $w = 0$, the overhead vanishes.
- **Classical post-processing:** Evaluate the generalized eigenvalue problem (size = number of check operators used); polynomial.

## Caveat

- Can only remove errors up to weight $d-1$ (strict projection) or $\lfloor(d-1)/2\rfloor$ (with recovery). Logical errors within the code space are invisible to projection alone.
- The sampling cost grows as the state gets noisier. At high error rates, you're throwing away most of your signal. This is error *mitigation*, not a path to fault tolerance.
- Multi-round performance (repeated noisy operations within the code) is not analyzed — the pseudo-thresholds quoted are for a single noise application.
- Non-transversal gates in this framework remain an open question.

## Related notes
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]]
- [[Stochastic Stabilizer Sampling for Post-Processed QEC]]
- [[Symmetry-Based QSE for Unencoded Error Mitigation]]
- [[Number-Basis Postselection for Error Mitigation]] — a simpler version using particle-number symmetry
- [[Symmetry Reduction in Qubit Hamiltonians]] — uses the same $Z_2$ symmetries for qubit reduction
- [[Variational Quantum Eigensolver (VQE)]]
