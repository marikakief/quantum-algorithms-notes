# Coherent Alias Sampling for PREPARE

> **Source:** Babbush, Gidney, Berry, Wiebe et al., arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Tags:** #trick #state-preparation #LCU #PREPARE #alias-sampling #T-complexity #oracle

## What it does

Prepares the LCU coefficient state $|L\rangle = \sum_\ell \sqrt{w_\ell/\lambda}\,|\ell\rangle$ — the PREPARE oracle for qubitization or LCU simulation — using T-gate count $O(L + \mu)$ where $\mu = O(\log(L/\varepsilon))$ precision bits. The key advantage over naive state preparation ($O(L)$ T gates for arbitrary states but with bad precision scaling) is that the precision dependence is *additive*, not multiplicative.

## The trick

**Problem:** For LCU simulation, PREPARE must create superpositions with amplitudes proportional to $\sqrt{w_\ell/\lambda}$. Arbitrary $L$-dimensional state preparation naively requires $O(L)$ T gates, but each amplitude must be specified to $\mu$ bits of precision, which can multiply the cost.

**Solution:** Use the classical *Vose alias method* (a linear-time classical algorithm for sampling from arbitrary discrete distributions) as a coherent quantum circuit.

**Classical alias method:** Given probabilities $\{\rho_\ell\}_{\ell=0}^{L-1}$ (summing to 1), precompute a table of pairs:
- $\text{alt}_\ell \in \{0,\ldots,L-1\}$: an alternate index
- $\text{keep}_\ell \in [0, 2^\mu)$: a threshold value scaled by $2^\mu$

The table satisfies: $\Pr[\text{output} = \ell] = \rho_\ell$ when you (i) draw $\ell$ uniformly from $\{0,\ldots,L-1\}$, (ii) draw $\sigma$ uniformly from $\{0,\ldots,2^\mu-1\}$, (iii) output $\ell$ if $\sigma < \text{keep}_\ell$, else output $\text{alt}_\ell$.

**Coherent version (Section III D, Eqs. 34–40, Fig. 13 of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush 2018]]):**

1. Prepare uniform superposition over $\ell \in \{0,\ldots,L-1\}$ (using [[Unary Iteration]] for non-power-of-two $L$, at cost $O(\log L)$ T gates via amplitude amplification).
2. Prepare uniform superposition over $\sigma \in \{0,\ldots,2^\mu-1\}$ (Hadamard gates, 0 T cost).
3. Use [[QROM (Quantum Read-Only Memory)]] to coherently load $\text{alt}_\ell$ and $\text{keep}_\ell$ for all $\ell$ simultaneously. T cost: $4L - 4$.
4. Compare $\sigma < \text{keep}_\ell$ via a quantum comparator circuit. T cost: $O(\mu)$.
5. Conditionally swap: if $\sigma < \text{keep}_\ell$, keep $\ell$; else replace $\ell$ with $\text{alt}_\ell$.

The output register $|\ell\rangle$ (after tracing out the $|\sigma, \text{alt}_\ell, \text{keep}_\ell\rangle$ ancilla) holds $\sqrt{\tilde{\rho}_\ell}\,|\ell\rangle$ as required, where $\tilde{\rho}_\ell = \lfloor 2^\mu \rho_\ell \rfloor / 2^\mu$ are the discretized probabilities.

**Precision:** Discretize $\rho_\ell = w_\ell/\lambda$ to $\mu$ bits where (Eq. 36):

$$\mu = \left\lceil \log\frac{\lambda N^2}{\varepsilon} \right\rceil$$

The resulting coefficient error feeds into the eigenvalue error; Appendix A of the source paper bounds the propagation. The T-gate cost for precision grows as $O(\mu) = O(\log(L/\varepsilon))$ — logarithmic in $1/\varepsilon$, not polynomial.

**Full `subprepare` cost (Fig. 15):**
- T gates: $6N + O(\mu + \log N)$ (dominated by two QROM calls, one for each of the two index dimensions in the chemistry Hamiltonian)
- Qubits: $2\mu + 3\log N + O(1)$

## When to reach for it

- Implementing PREPARE for [[Qubitization (Quantum Walk for Spectral Encoding)]] or LCU simulation.
- Any situation where you need to prepare a state with $L$ amplitudes proportional to $\sqrt{w_\ell}$, where the $w_\ell$ are known classically.
- When $L$ is large ($O(N^2)$ or more) and you want T-gate cost linear in $L$ with only logarithmic precision overhead.
- Chemistry and Hubbard Hamiltonians in the plane-wave dual basis (where $L = O(N^2)$ for the number of distinct coefficient magnitudes is smaller due to translational symmetry).

## Complexity

- T gates: $O(L + \mu)$ where $\mu = O(\log(L/\varepsilon))$
- Ancilla qubits: $O(\mu + \log L)$
- Classical preprocessing: $O(L)$ to compute alias tables

## Caveat

- Requires classical precomputation of alias tables for the specific probability distribution. Not adaptive — if the Hamiltonian coefficients change, the tables (and hence the circuit) must be recomputed.
- The "garbage" ancilla $|\text{alt}_\ell, \text{keep}_\ell, \sigma\rangle$ must be uncomputed before PREPARE$^\dagger$ is applied. This uncomputation is free (measure and reset classically), but requires the garbage register to be kept around until needed.
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
