> **Source:** Jarrod R. McClean, Zhang Jiang, Nicholas C. Rubin, Ryan Babbush, Hartmut Neven, *Decoding quantum errors with subspace expansions*, arXiv:1903.05786, Nature Communications **11**, 636 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1903.05786) · [Journal](https://doi.org/10.1038/s41467-020-14341-w)
> **Tags:** #error-mitigation #quantum-error-correction #subspace-expansion #stabilizer-codes #NISQ #symmetry-verification #post-processing

---

## The computational problem

Given a noisy quantum state $\rho$ that was intended to be a code state $|\Psi\rangle$ (or an eigenstate of a problem Hamiltonian), recover the expectation value of a logical observable $\Gamma$ without active syndrome measurement, ancilla qubits, or fast feedback. All correction is done via additional Pauli measurements and classical post-processing.

## What the paper does

Shows that quantum error correcting codes can be studied and *used* on NISQ devices by replacing syndrome measurement + recovery with post-processed stabilizer projections. The key idea: construct projectors $P = \prod_i (I + S_i)/2$ from the code's stabilizer generators and use them to project out non-code-space components of $\rho$ entirely in post-processing. Since $P$ decomposes into a sum of Pauli operators (all stabilizer group elements), the corrected expectation value $\text{Tr}(P\rho P \Gamma) / \text{Tr}(P\rho P)$ requires only measuring products of Pauli operators — something any NISQ device can do.

This is then generalized in three directions:
1. **With recovery operations** — projecting into correctable error subspaces and applying known recovery maps, increasing the effective signal at the cost of potentially introducing logical errors.
2. **Subspace expansion** — relaxing the uniform-coefficient projector into an optimized linear combination $P_c = \sum_i c_i M_i$, solved via a generalized eigenvalue problem (the same [[Virtual Quantum Subspace Expansion (VQSE)|QSE framework]] used for basis extension in chemistry).
3. **Unencoded Hamiltonians** — applying the same machinery to physical symmetries ($Z_2$ parities, approximate occupation symmetries) of an unencoded problem Hamiltonian, with no error-correcting code at all.

The `[[5,1,3]]` code demonstration achieves a pseudo-threshold of $p \approx 0.50$ under single-qubit depolarizing noise. The unencoded H₂ demonstration shows improvement across the entire error range.

## The algorithm / construction

### Stabilizer projection (basic version)

For a stabilizer code with generators $S = \{S_1, \ldots, S_m\}$, the code-space projector is:

$$P = \prod_{i=1}^{m} \frac{I + S_i}{2} = \frac{1}{2^m} \sum_{M_i \in \mathcal{S}} M_i$$

where $\mathcal{S}$ is the full stabilizer group ($2^m$ elements). The corrected expectation value of a logical observable $\Gamma = \sum_j \gamma_j \Gamma_j$ (Pauli decomposition) is:

$$\langle \Gamma \rangle_\text{corr} = \frac{\text{Tr}(P\rho P \Gamma)}{\text{Tr}(P\rho P)} = \frac{1}{c} \cdot \frac{1}{2^m} \sum_{j,k} \gamma_j \,\text{Tr}(\rho\, \Gamma_j M_k)$$

where $c = \text{Tr}(P\rho P)$ is the normalization. The simplification uses: (a) $P = P^\dagger = P^2$; (b) logical operators commute with stabilizer elements; (c) if $M_i^\dagger M_k \in \mathcal{S}$, the double sum collapses to a single sum.

Each term $\text{Tr}(\rho\, \Gamma_j M_k)$ is just a Pauli expectation value — measurable by preparing $\rho$ and measuring in the appropriate Pauli basis. No ancilla or syndrome extraction needed. The state is destroyed each shot, which is fine because this is post-processing.

### Stochastic sampling

The sum has $2^m$ terms from the stabilizer group, but a [[Stochastic Stabilizer Sampling for Post-Processed QEC|stochastic sampling scheme]] avoids enumerating all of them. Sample stabilizer group elements $S_\chi = S_1^{\chi_1} S_2^{\chi_2} \cdots S_m^{\chi_m}$ (indexed by bitstring $\chi \in \{0,1\}^m$) and Pauli terms $\Gamma_j$ with probability $p_{\chi,j} = \gamma_j / 2^m$. Each sample produces a $\pm 1$ measurement outcome.

The variance of this estimator depends on the *quality* of the state, not the number of stabilizer terms:
- For a perfect code state ($\rho = |\Psi\rangle\langle\Psi|$): zero additional variance beyond the original measurement of $\Gamma$.
- For the depolarizing channel $\rho = (1-w)|\Psi\rangle\langle\Psi| + \frac{w}{2^n}I$: variance $\sim \frac{1}{4}(2-w)w$ for measuring an eigenstate of $\Gamma$.

This is the key practical feature: the cost scales with the error rate, not with the code size.

### Projection with recovery

For a correctable error $E_\alpha$ with known syndrome $s_{\alpha,j}$, project into the error subspace:

$$P_{E_\alpha} = \prod_j \frac{1}{2}\left(I + (-1)^{s_{\alpha,j}} S_j\right)$$

then apply recovery $R_\alpha$ to map back to the code space. The corrected observable becomes:

$$\langle \Gamma \rangle = \frac{1}{c} \sum_\alpha \text{Tr}(R_\alpha P_{E_\alpha} \rho P_{E_\alpha} R_\alpha^\dagger \Gamma)$$

**Trade-off:** Recovery increases the effective signal (more of the state contributes) but can introduce logical errors when the actual error exceeds the correctable set. Strict projection (without recovery) can remove errors up to weight $d-1$; recovery can only handle up to weight $\lfloor(d-1)/2\rfloor$.

### Subspace expansion (QSE generalization)

Relax from exact projection to optimal linear combination. Use check operators $\{M_i\}$ from the stabilizer group with free coefficients $c_i$, and minimize the energy of the code Hamiltonian $H_c = -\sum M_i$ subject to normalization. This yields the generalized eigenvalue problem:

$$HC = SCE$$

$$H_{ij} = \text{Tr}(M_i^\dagger H_c M_j \rho), \qquad S_{ij} = \text{Tr}(M_i^\dagger M_j \rho)$$

This is exactly the [[Virtual Quantum Subspace Expansion (VQSE)|QSE framework]] from McClean et al. (2017), now applied to error correction rather than basis extension. The measurement cost is *linear* in the number of check operators (not quadratic) because the commutation $[M_i, H_c] = 0$ collapses $M_i^\dagger H_c M_j$ to $H_c M_k$ for some single $M_k$.

When combined with a problem Hamiltonian $H_p$ expressed in logical operators, the subspace expansion on $H_c + H_p$ can also correct logical errors — the problem Hamiltonian breaks the code-space degeneracy and selects the correct logical state.

### Application to unencoded Hamiltonians

For a physical Hamiltonian $H_p$ (no error-correcting encoding), use known symmetries as "stabilizers":

- **$Z_2$ parities:** Particle number parity $\prod_i Z_i$, spin-up parity $\prod_{i \in \alpha} Z_i$, spin-down parity $\prod_{i \in \beta} Z_i$ in Jordan-Wigner encoding. These are exact symmetries with eigenvalues $\pm 1$, directly usable as projectors.
- **Approximate symmetries:** Local occupation operators $Z_i$ (single-site parity) aren't exact symmetries, but the QSE procedure can automatically decide whether to project — balancing the energetic cost of discarding the correct component against the noise reduction from removing erroneously occupied high-energy orbitals.
- **Pair occupation projectors:** $(I + Z_i Z_j)$ for pairs of high-energy orbitals — removes correlated occupation errors that can't be caught by single-qubit projectors.

The QSE approach is especially useful here: if you know a $Z_2$ symmetry $F$ but not which eigenspace the ground state belongs to, the subspace expansion will select the correct one automatically.

## Key results

**`[[5,1,3]]` code, depolarizing channel on all 5 qubits:**
- Full stabilizer projection ($l=4$ generator products): pseudo-threshold $p \approx 0.50$. Below this error rate, the encoded+projected logical fidelity exceeds the unencoded physical fidelity.
- The QSE relaxation (fewer check operators) interpolates smoothly between projection levels — removing 2 operators and re-optimizing gives performance between adjacent exact projection levels.
- Logical $X$ gate (transversal): pseudo-threshold $p \approx 0.45$.

**H₂ (unencoded, 4 qubits, STO-3G, stretched geometry):**
- Using $S = \{Z_0 Z_2, Z_1 Z_3, X_0 X_1 X_2 X_3\}$ (spin parities + a non-local operator that need not be an exact symmetry):
- Up to $\sim 3\times$ improvement in logical infidelity across the entire depolarizing range.
- No encoding overhead, so the improvement is always positive — no pseudo-threshold to cross.

**Sampling cost:** For a state with depolarizing error probability $w$, the variance of the corrected estimator scales as $\sim w(2-w)/4$. At $w=0$, the correction adds zero overhead. The cost grows with noise, not with code size.

## Comparison with prior work

| Method | Ancilla needed | Syndrome measurement | Fast feedback | Corrects non-code errors | Corrects logical errors | Works without encoding |
|---|---|---|---|---|---|---|
| Standard QEC (stabilizer) | Yes | Yes | Yes | Yes (active) | No | No |
| Error detection + postselection | Yes (for syndrome) | Yes | No | Partial (discard) | No | No |
| Symmetry verification (Bonet-Monroig et al. 2018) | No | No | No | $Z_2$ parity only | No | Yes |
| [[Number-Basis Postselection for Error Mitigation\|Number-basis postselection]] | No | No | No | Number/spin sector | No | Yes (chemistry) |
| Zero-noise extrapolation (Temme et al. 2017) | No | No | No | All (extrapolated) | All (extrapolated) | Yes |
| **This paper (projection)** | **No** | **No** | **No** | **Up to weight $d-1$** | **No** | **Yes (with symmetries)** |
| **This paper (QSE + $H_p$)** | **No** | **No** | **No** | **Up to weight $d-1$** | **Some** | **Yes** |

The symmetry verification work (Bonet-Monroig et al.) is a special case of this framework — it uses individual $Z_2$ symmetry projectors without the QSE optimization step. This paper generalizes it to arbitrary stabilizer codes, recovery operations, and optimized subspace expansions.

## Limits / caveats

- **Not a path to fault tolerance.** The sampling cost grows with noise rate, and the method can't correct all errors — it's error *mitigation*, not error *correction* in the scalable sense. As the authors note, this is for studying codes and enabling near-term applications, not for building a fault-tolerant computer.
- **Pseudo-threshold vs real threshold.** The $p \approx 0.50$ number is for a single round of noise on the `[[5,1,3]]` code. With repeated operations (actual computation), the errors accumulate and the effective threshold drops. The paper doesn't analyze multi-round performance.
- **Non-transversal gates.** The logical $X$ gate is transversal in the `[[5,1,3]]` code, so it's easy. The paper leaves open how to handle non-transversal gates, which is where things get hard in any code.
- **Sampling overhead in practice.** The variance analysis assumes uniform sampling. Importance sampling (weighting by Pauli weight of $S_\chi$) can help, but the paper doesn't quantify the improvement beyond the prescription.
- **H₂ is H₂.** The unencoded demonstration is on a 4-qubit system. Whether the symmetry-based approach scales to larger molecules with many approximate symmetries is unclear. My assessment: the exact $Z_2$ symmetries (number parity, spin parity) are always worth including — they're cheap and guaranteed to help. The approximate symmetries (orbital occupation) are where the engineering judgment comes in, and the QSE optimization is genuinely useful there.
- **Compatible with extrapolation.** The paper notes this method composes with zero-noise extrapolation — apply extrapolation to each matrix element, then solve the QSE problem. This is nice but unexplored numerically.

## Reusable ideas

1. [[Stabilizer Subspace Projection for Error Mitigation]] — Use stabilizer group elements as post-processing projectors to remove non-code-space components of a noisy state. No ancilla, no syndrome measurement — just additional Pauli measurements.

2. [[Stochastic Stabilizer Sampling for Post-Processed QEC]] — Sample from the stabilizer group stochastically rather than enumerating all $2^m$ terms. The variance depends on the state quality (how much is outside the code space), not on the code size.

3. [[Symmetry-Based QSE for Unencoded Error Mitigation]] — Apply the QSE framework to $Z_2$ symmetries and approximate symmetries of an unencoded problem Hamiltonian. The generalized eigenvalue problem automatically selects which projections help and which hurt.

## References within this paper

- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — VQE on H₂; the same molecule used as the unencoded test case here.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — $N$-representability constraints for error mitigation in fermionic problems; cited as a specialized version of symmetry-based error reduction.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — VQE with UCC ansatz; context for variational algorithms that benefit from error mitigation.
- McClean, Kimchi-Schwartz, Carter, de Jong (2017), Phys. Rev. A **95**, 042308 — Original QSE proposal for error mitigation and excited states. This paper generalizes QSE to stabilizer codes and recovery operations.
- Colless et al. (2018), Phys. Rev. X **8**, 011021 — Experimental demonstration of QSE for molecular spectra. The "error-resilient algorithm" that confirmed QSE works on hardware.
- Bonet-Monroig, Sagastizabal, Singh, O'Brien (2018), Phys. Rev. A **98**, 062339 — Symmetry verification: projects onto the correct parity sector. A special case of the framework developed here.
- Temme, Bravyi, Gambetta (2017), PRL **119**, 180509 — Zero-noise extrapolation. Compatible with the subspace expansion approach (apply to matrix elements).
- Endo, Benjamin, Li (2018), Phys. Rev. X **8**, 031027 — Practical error mitigation via extrapolation; another compatible technique.
- Gottesman (1997) — Stabilizer formalism. The theoretical backbone of the code-based constructions.
- Bravyi, Gambetta, Mezzacapo, Temme (2017) — Tapering off qubits: identifies $Z_2$ symmetries for qubit reduction. The same symmetries are reused here as projectors instead.
- Preskill (2018) — NISQ era framing.
- Motta, Ye, [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean et al. (2019)]] — Quantum imaginary time evolution and quantum Lanczos; cited as a QSE variation.

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE proposal; the stabilizer subspace projection here extends VQE's error-resilience from passive (variational principle) to active (symmetry-based postselection)
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Theoretical companion introducing variational error suppression; this paper provides a more systematic error mitigation via stabilizer projections and QSE
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — Uses the same QSE framework for a different purpose (basis extension via [[Virtual Quantum Subspace Expansion (VQSE)|VQSE]]). Published the same month (March 2019) — these are companion papers exploring two faces of QSE.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — $N$-representability as a domain-specific form of symmetry projection for error mitigation.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — VQE on H₂; the unencoded test case here.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — [[Number-Basis Postselection for Error Mitigation|Number-basis postselection]] is a related symmetry-based error mitigation approach.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — VQE context; benefits from the error mitigation developed here.
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov, Sun, Babbush, Chan et al 2022]]

### Trick cards
- [[Stabilizer Subspace Projection for Error Mitigation]] — Main trick from this paper
- [[Stochastic Stabilizer Sampling for Post-Processed QEC]] — Efficient sampling trick from this paper
- [[Symmetry-Based QSE for Unencoded Error Mitigation]] — Unencoded application trick from this paper
- [[Virtual Quantum Subspace Expansion (VQSE)]] — Same QSE mathematical framework, applied to basis extension instead of error correction
- [[Number-Basis Postselection for Error Mitigation]] — Related symmetry-based error mitigation
- [[Symmetry Reduction in Qubit Hamiltonians]] — Same $Z_2$ symmetries, used for qubit reduction instead of error mitigation
- [[Variational Quantum Eigensolver (VQE)]] — Primary application context
- [[2-RDM Physicality Restoration by PSD Projection]] — Another post-processing error mitigation approach for fermionic systems
