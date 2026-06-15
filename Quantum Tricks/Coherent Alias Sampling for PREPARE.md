
> **Source:** Babbush, Gidney, Berry, Wiebe et al., arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Tags:** #trick #state-preparation #LCU #PREPARE #alias-sampling #T-complexity #oracle

## What it does

Prepares the coherent LCU coefficient state used by PREPARE oracles, with amplitudes whose marginal distribution over the selected index is proportional to $w_\ell/\lambda$. In the 2018 plane-wave-dual chemistry construction this gives T-gate count $O(L+\mu)$ with an additive precision-bit cost $\mu$, rather than multiplying the table-lookup cost by the number of precision bits.

## The trick

**Problem:** For LCU simulation, PREPARE must create superpositions with amplitudes proportional to $\sqrt{w_\ell/\lambda}$. Arbitrary $L$-dimensional state preparation naively requires $O(L)$ T gates, but each amplitude must be specified to $\mu$ bits of precision, which can multiply the cost.

**Solution:** Use the classical *Vose alias method* (a linear-time classical algorithm for sampling from arbitrary discrete distributions) as a coherent quantum circuit.

**Classical alias method:** Given probabilities $\{\rho_\ell\}_{\ell=0}^{L-1}$ (summing to 1), precompute a table of pairs:
- $\text{alt}_\ell \in \{0,\ldots,L-1\}$: an alternate index
- $\text{keep}_\ell \in [0, 2^\mu)$: a threshold value scaled by $2^\mu$

The table satisfies: $\Pr[\text{output} = \ell] = \rho_\ell$ when you (i) draw $\ell$ uniformly from $\{0,\ldots,L-1\}$, (ii) draw $\sigma$ uniformly from $\{0,\ldots,2^\mu-1\}$, (iii) output $\ell$ if $\sigma < \text{keep}_\ell$, else output $\text{alt}_\ell$.

**Coherent version (Section III D, Eqs. 34–40, Fig. 13 of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush 2018]]):**

1. Prepare a uniform superposition over $\ell \in \{0,\ldots,L-1\}$. The exact method and cost for non-power-of-two $L$ are implementation-dependent and should not be identified with the unary-iteration SELECT primitive.
2. Prepare uniform superposition over $\sigma \in \{0,\ldots,2^\mu-1\}$ (Hadamard gates, 0 T cost).
3. Use [[QROM (Quantum Read-Only Memory)]] to coherently load $\text{alt}_\ell$ and $\text{keep}_\ell$ for all $\ell$ simultaneously. T cost: $4L - 4$.
4. Compare $\sigma < \text{keep}_\ell$ via a quantum comparator circuit. T cost: $O(\mu)$.
5. Conditionally swap: if $\sigma < \text{keep}_\ell$, keep $\ell$; else replace $\ell$ with $\text{alt}_\ell$.

The full output is a pure coherent state over the selected index and auxiliary registers. Its marginal distribution over the selected index has probabilities $\tilde{\rho}_\ell$, where $\tilde{\rho}_\ell = \lfloor 2^\mu \rho_\ell \rfloor / 2^\mu$ are the discretized probabilities. For qubitization, the reflection/PREPARE$^\dagger$ must act on this full coherent state; one cannot trace out the auxiliary registers and still have a clean amplitude state.

**Precision in the 2018 chemistry construction:** Discretize $\rho_\ell = w_\ell/\lambda$ to $\mu$ bits where (Eq. 36):

$$\mu = \left\lceil \log\frac{\lambda N^2}{\varepsilon} \right\rceil$$

The resulting coefficient error feeds into the eigenvalue error; Appendix A of the source paper bounds the propagation. The T-gate cost for precision grows additively as $O(\mu)$ in that implementation. Other PREPARE constructions may have different precision dependence.

**Full `subprepare` cost (Fig. 15):**
- T gates: $6N + O(\mu + \log N)$ (dominated by two QROM calls, one for each of the two index dimensions in the chemistry Hamiltonian)
- Qubits: $2\mu + 3\log N + O(1)$

## When to reach for it

- Implementing PREPARE for [[Qubitization (Quantum Walk for Spectral Encoding)]] or LCU simulation.
- Any situation where you need to prepare a state with $L$ amplitudes proportional to $\sqrt{w_\ell}$, where the $w_\ell$ are known classically.
- When $L$ is large ($O(N^2)$ or more) and you want T-gate cost linear in $L$ with only logarithmic precision overhead.
- Chemistry and Hubbard Hamiltonians in the plane-wave dual basis (where $L = O(N^2)$ for the number of distinct coefficient magnitudes is smaller due to translational symmetry).

## Complexity

- T gates: $O(L + \mu)$ in the 2018 alias-QROM construction, where $\mu$ is the chosen additive precision
- Ancilla qubits: $O(\mu + \log L)$
- Classical preprocessing: $O(L)$ to compute alias tables

## Caveat

- Requires classical precomputation of alias tables for the specific probability distribution. Not adaptive — if the Hamiltonian coefficients change, the tables (and hence the circuit) must be recomputed.
- The auxiliary registers $|\text{alt}_\ell, \text{keep}_\ell, \sigma\rangle$ are part of the coherent PREPARE state. They must be handled by the same PREPARE/reflection/PREPARE$^\dagger$ construction, or by a compatible measurement-based cleanup only after their coherent contents are no longer needed.
- For general state preparation beyond LCU (e.g., preparing complex-amplitude states), the method needs to be adapted to handle signs and phases separately from magnitudes.

## Related notes

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — introduced here
- [[QROM (Quantum Read-Only Memory)]] — used inside alias sampling to load the tables
- [[Unary Iteration]] — used to create the uniform superposition over non-power-of-two $L$
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the PREPARE oracle for qubitization uses this trick
- [[Truncated Taylor Series Simulation]] — the LCU framework that also requires a PREPARE oracle; this trick applies there too
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — extends alias sampling to the sparse Coulomb case: QROAM iterates over nonzero entries and outputs both the coefficient index and the alias/keep values, with controlled swaps generating symmetry copies; introduced alongside [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — uses coherent alias sampling for PREPARE over the THC index pair $(\mu,\nu)$ with amplitudes $\sqrt{|\zeta_{\mu\nu}|/\lambda_\zeta}$; the standard trick from Babbush 2018 applied to the THC coefficient distribution
- [[Asymmetric Qubitization]] — asymmetric qubitization (SYK paper) replaces alias sampling for PREPARE with random circuits; used when the coefficient distribution is Gaussian and random circuits are cheaper than explicit alias tables
