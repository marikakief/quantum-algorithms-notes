
> **Source:** Huggins, McClean, Rubin, Jiang, Wiebe, Whaley, Babbush, arXiv:1907.13117
> **Tags:** #trick #error-mitigation #postselection #measurement #NISQ #quantum-chemistry #symmetry-verification

## What it does

When all Hamiltonian measurements are performed in the occupation number basis (after appropriate single-particle rotations), each measurement shot simultaneously yields the total particle number $\eta$ and spin projection $S_z$. Discarding shots with incorrect $\eta$ or $S_z$ projects the measured state onto the correct symmetry sector, mitigating both circuit errors and readout errors at zero additional circuit cost.

## The trick

After applying a single-particle basis rotation $U_\ell^\dagger$ and measuring in the computational basis, each bitstring $b = (b_1, \ldots, b_N)$ directly encodes occupation numbers under Jordan-Wigner. From any bitstring:

$$\eta_\text{measured} = \sum_p b_p, \qquad S_{z,\text{measured}} = \frac{1}{2}\left(\sum_{p \in \uparrow} b_p - \sum_{p \in \downarrow} b_p\right)$$

**Postselection:** Keep only those bitstrings where $\eta_\text{measured} = \eta_\text{target}$ and $S_{z,\text{measured}} = S_{z,\text{target}}$. The retained data estimates the projected expectation value:

$$\langle H \rangle_\text{proj} = \frac{\mathrm{Tr}(P\rho H)}{\mathrm{Tr}(P\rho)}$$

where $P$ projects onto the $(\eta, S_z)$ eigenspace.

**Why this is stronger than parity-based methods:** Prior error mitigation approaches (Bonet-Monroig et al. 2018; McArdle et al. 2019) project onto the correct parity of $\eta$ and $S_z$ (even vs odd), which catches only errors that flip an even number of particles to odd or vice versa. Number-basis postselection catches *any* error that changes the particle count — including single-qubit errors that add or remove one particle.

**Readout error resilience:** Under a symmetric bitflip channel with single-qubit error rate $p$, a Pauli operator on $K$ qubits gets suppressed by $(1-2p)^K$. Standard Jordan-Wigner Pauli terms have $K$ up to $N$, giving exponential suppression. In the number basis, all operators have $K \leq 2$ (they're $n_p$ or $n_p n_q$), so suppression is at most $(1-2p)^2$ — independent of system size.

**Cost:** The overhead is $\approx 1/\mathrm{Tr}(P\rho)$ additional measurements (inverse probability of landing in the correct sector). This is partially offset because removing wrong-sector shots also reduces variance.

## When to reach for it

- Whenever measurements are performed in the [[Basis Rotation Grouping for VQE Measurement|basis rotation grouping]] scheme (all measurements are automatically number-diagonal).
- Any measurement protocol where intermediate basis rotations produce diagonal operators — the postselection comes for free.
- As a complement to (not replacement for) zero-noise extrapolation: the paper notes these approaches combine favourably.
- Particularly effective when readout error is the dominant noise channel.

## Complexity

- **Additional circuit cost:** Zero. The postselection is applied purely in classical post-processing.
- **Measurement overhead:** $\sim 1/\mathrm{Tr}(P\rho)$ factor in shot count.
- **Classical post-processing:** Trivial — just count $1$s in the bitstring and discard wrong-sector shots.

## Caveat

- Only works when measurements are in the number basis. Standard Pauli-term measurements (non-diagonal) don't have this structure.
- The postselection probability $\mathrm{Tr}(P\rho)$ decreases with increasing noise, so at very high error rates the overhead becomes substantial.
- Doesn't correct errors that stay within the correct $(\eta, S_z)$ sector — it's a filter, not a full error correction scheme.
- The [[Basis Rotation Grouping for VQE Measurement|basis rotation circuit]] itself introduces errors that may push the state out of the correct sector, partially defeating the purpose. The paper's simulations show net benefit for gate error rates $\lesssim 10^{-2}$.

## Related notes
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]]
- [[Basis Rotation Grouping for VQE Measurement]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[2-RDM Physicality Restoration by PSD Projection]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Stabilizer Subspace Projection for Error Mitigation]] — generalization from particle-number symmetry to arbitrary stabilizer codes
- [[Symmetry-Based QSE for Unencoded Error Mitigation]] — QSE-optimized version of symmetry projection
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]]
