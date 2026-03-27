
> **Source:** Su, Berry, Wiebe, Rubin, Babbush, arXiv:2105.12767 (2021)
> **Tags:** #trick #LCU #kinetic-energy #arithmetic-free #first-quantized #qubitization

## What it does

Eliminates all multiplication circuits from the block encoding of a kinetic energy operator by decomposing the squared momentum $\|p\|^2$ into a sum of single-bit products, each implementable with one Toffoli gate.

## The trick

The kinetic energy in a first-quantized plane-wave basis is

$$T = \frac{2\pi^2}{\Omega^{2/3}} \sum_{j=1}^\eta \sum_{p \in G} \|p\|^2 |p\rangle\langle p|_j$$

Normally, computing $\|p\|^2$ requires squaring $n_p$-bit numbers ($O(n_p^2)$ Toffolis per electron). Instead, write $\|p\|^2 = \sum_{w \in \{x,y,z\}} \sum_{r,s} 2^{r+s} p_{w,r} \cdot p_{w,s}$ where $p_{w,r}$ is bit $r$ of momentum component $w$. Each bit product $p_{w,r} \cdot p_{w,s}$ takes the values 0 or 1. Use the identity

$$p_{w,r} \cdot p_{w,s} = \frac{1 - (-1)^{p_{w,r} \cdot p_{w,s} \oplus 1}}{2}$$

to write $T$ as an LCU where each unitary just applies a $(-1)$ phase conditioned on two bits of the momentum register. The PREPARE oracle creates a weighted superposition $\sum_{r,s} 2^{(r+s)/2}|r\rangle|s\rangle$ (using cascading controlled Hadamards, $n_p - 2$ Toffolis). The SELECT oracle computes the bit product in an ancilla and does a controlled-Z on a $|+\rangle$ state — one Toffoli total, independent of system size.

The SEL cost for $T$ is only $5(n_p - 1) + 2$ Toffolis (to copy the relevant bits and compute the product), compared to $O(\eta n_p^2)$ for direct squaring.

## When to reach for it

- Block encoding any diagonal operator that involves squares of integer-valued registers (e.g., kinetic energy in a discrete momentum basis)
- Any setting where multiplication is the dominant cost and the operator is a sum of products of individual bits of the input register
- Most valuable when the operator must be block-encoded many times (as in [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] where $O(\lambda/\varepsilon)$ calls are needed)

## Complexity

- PREPARE for $r, s$: $2(n_p - 2)$ Toffolis (preparation + inverse)
- SELECT for $T$: $5(n_p - 1) + 2$ Toffolis
- Compare to naive: $O(\eta n_p^2)$ Toffolis for arithmetic squaring — the savings are a factor of $\sim \eta n_p$

## Caveat

Only works when $T$ is diagonal in the computational basis and the squared quantity is stored as a binary integer. If the kinetic energy involves non-orthogonal Bravais vectors (non-cubic lattices), the squared norm $\|k_p\|^2$ becomes a sum of cross-products of different momentum components, breaking the clean bit-product decomposition. The paper flags non-cubic lattices as future work.

Also, this trick applies to the qubitization approach only. In the interaction picture, $e^{-iT\tau}$ requires the actual numerical value of the kinetic energy for the phase rotation, so the bit-product LCU structure doesn't help directly — though the [[Incremental Kinetic Energy Register]] trick addresses that cost instead.

## Related notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Incremental Kinetic Energy Register]] — the complementary trick for the interaction picture
- [[Unary Iteration]] — used to iterate over the $\eta$ momentum registers for the controlled swap
