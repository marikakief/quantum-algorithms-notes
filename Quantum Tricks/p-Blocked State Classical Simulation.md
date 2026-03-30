# p-Blocked State Classical Simulation

> **Source:** R. Jozsa, N. Linden, arXiv:quant-ph/0201143
> **Tags:** #trick #classical-simulation #entanglement #p-blocked #complexity

## What it does

Efficiently classically simulates any quantum computation whose state remains $p$-blocked (factorisable into blocks of at most $p$ mutually entangled qubits) at every step, for any fixed $p$.

## The trick

A $p$-blocked pure state of $m$ qubits is a tensor product across a partition into blocks of size $\leq p$:

$$|\alpha\rangle = |\phi_1\rangle_{B_1} \otimes \cdots \otimes |\phi_K\rangle_{B_K}, \quad |B_i| \leq p$$

This has $O(m \cdot 2^p)$ parameters — polynomial in $m$ for fixed $p$.

The simulation maintains this compact description through each gate:

1. **Same-block gate:** The gate acts within one block. Update the block's $\leq 2^p$-dimensional state vector. Cost: $O(2^{2p})$ arithmetic operations (constant for fixed $p$).

2. **Cross-block gate:** The gate straddles two blocks $B_1, B_2$ with $|B_1| + |B_2| \leq 2p$ qubits.
   - Merge the two blocks into a single $\leq 2^{2p}$-dimensional state
   - Apply the gate
   - Re-decompose: check all partitions of the merged block to find one that is $p$-blocked (comparing the joint state against products of reduced states)
   - Cost: constant (exponential in $p$ but $p$ is fixed)

The $p$-blocked decomposition must exist at each step by assumption. Finding it requires checking $O(2^{2p})$ partitions — constant for fixed $p$.

For approximate $p$-blocking (state within distance $\varepsilon$ of a $p$-blocked state), the simulation introduces error $O((2^p + 4)^T \cdot \varepsilon)$ after $T$ steps. Keeping this below tolerance $\eta$ requires $\varepsilon \leq \eta \cdot (2^p + 4)^{-T}$.

## When to reach for it

- When analysing whether a quantum algorithm's speedup is "real" or an artifact of limited classical simulation effort
- When designing variational ansätze: if the ansatz only generates $p$-blocked states, it can be classically simulated — so it can't provide exponential speedup
- When proving that a specific quantum algorithm requires large entanglement (by contradiction: if the algorithm only creates $O(\log n)$-blocked states, it's classically efficient)
- Tensor network methods (MPS, PEPS) are a practical generalisation: they bound entanglement in a different way (Schmidt rank rather than block size) but the philosophy is the same

## Complexity

$O(T \cdot 2^{O(p)} \cdot \mathrm{poly}(m))$ for a $T$-step computation on $m$ qubits. The exponential in $p$ is the fixed constant cost per gate. For the approximate case, add $\mathrm{poly}(\log(1/\eta))$ for the rational arithmetic precision.

## Caveat

The tolerance bound in the approximate case is exponentially tight in $T$. This means the simulation breaks down if *any* small entanglement accumulates over many steps — which is the generic situation. So the theorem is mainly useful as a proof tool (contrapositive: speedup implies entanglement) rather than as a practical classical simulation algorithm.

Also, the result says nothing about mixed-state computations. [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|DQC1]] operates on separable (but not $p$-blocked) mixed states and may still be classically hard.

## Related notes
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Entanglement as Computational Resource Lower Bound]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]
