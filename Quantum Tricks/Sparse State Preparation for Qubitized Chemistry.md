
> **Source:** Berry, Gidney, Motta, McClean, Babbush, arXiv:1902.02134 (Section 5)
> **Tags:** #trick #state-preparation #LCU #PREPARE #sparsity #qubitization #quantum-chemistry

## What it does
Modifies [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] so that PREPARE cost scales with the number of nonzero Hamiltonian coefficients rather than the full tensor dimension. For chemistry Hamiltonians with empirical sparsity in the Coulomb operator, this can reduce the state preparation cost by an order of magnitude.

## The trick
Standard [[Coherent Alias Sampling for PREPARE|alias sampling]] iterates over all $d$ possible coefficient indices (e.g., $d = N^4/16$ for a full two-electron tensor). If many coefficients are zero (or below a threshold $c$), this wastes most of the QROAM budget on zero entries.

**Modified procedure:** Let $\bar{d}$ be the number of nonzero entries.

1. Create an equal superposition over $\bar{d}$ values: $\frac{1}{\sqrt{\bar{d}}}\sum_{j=1}^{\bar{d}} |j\rangle$
2. Use [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] indexed on this register to output **three things**: the actual coefficient index $\mathrm{ind}_j$ (the $(p,q,r,s)$ tuple), an alternate index $\mathrm{alt}_j$, and a keep probability $\mathrm{keep}_j$.
3. Perform the alias sampling inequality test and conditional swap as usual.

The desired state appears in the $\mathrm{ind}$ register. Correctness follows directly from the standard alias sampling argument — after the QROAM lookup, the state in all registers except the first is equivalent to what standard alias sampling would produce.

**Symmetry exploitation:** For the chemistry Coulomb operator, eightfold symmetry ($V_{pqrs} = V_{qprs} = V_{pqsr} = V_{rspq} = \ldots$) means only $\sim \bar{d}/8$ unique entries need to be stored. After the sparse QROAM lookup, three controlled swaps generate the symmetry copies:
1. Swap $p \leftrightarrow q$ controlled on a Hadamard ancilla
2. Swap $r \leftrightarrow s$ controlled on another Hadamard ancilla
3. Swap the $(p,q)$ and $(r,s)$ register pairs controlled on a third Hadamard ancilla

This reduces the QROAM table size by $\sim 8\times$ while correctly producing all symmetry-related amplitudes.

**Threshold truncation:** Set small $V_{pqrs}$ entries to zero:

$$\tilde{V}^{(c)}_{pqrs} = \begin{cases} V_{pqrs} & |V_{pqrs}| \geq c \\ 0 & \text{otherwise} \end{cases}$$

Choose $c$ by monitoring CISD/MP2 correlation energies. The truncation reduces $\bar{d}$ without introducing chemical-accuracy errors. For FeMoCo (LLDUC): $c = 10^{-4}$ a.u. reduces the entries from $\sim 33$ million to $\sim 1.3$ million.

## When to reach for it
- Any qubitized chemistry simulation where the Coulomb tensor has empirical sparsity (most molecular systems in localized or symmetry-adapted orbital bases)
- When the non-factorized Hamiltonian gives a smaller $\lambda$ than the factorized form ($\lambda_V < \lambda_W$), making direct simulation preferable despite the larger coefficient count
- Works with any LCU-based simulation, not just qubitization

## Complexity
- QROAM cost: scales with $\bar{d}/8$ (unique nonzero entries after symmetry) instead of $N^4/16$
- Total PREPARE: $O(\sqrt{\bar{d} M / 8})$ Toffolis with clean ancillae
- The QROAM output size is larger than the factorized case ($M \approx 8\lceil\log(N/2)\rceil + \mu + 4$) because both ind and alt values need full $(p,q,r,s)$ tuples

## Caveat
The sparsity is not an asymptotic improvement: $\bar{d} = O(N^4)$ in general. The benefit is entirely from the constant-factor sparsity of practical molecular Hamiltonians. For extended systems or continuum-limit calculations, sparsity diminishes.

The QROAM output width $M$ is roughly doubled compared to the factorized case (since both the index and the alternate index must include full orbital labels), partially offsetting the benefit of smaller table size.

## Related notes
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Coherent Alias Sampling for PREPARE]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Single Factorization for Qubitized Chemistry]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
