
> **Source:** Kempe, Kitaev & Regev, arXiv:quant-ph/0406180, SICOMP 35(5) (2006)
> **Tags:** #trick #perturbation #2-local #QMA #gadget

## What it does

Replaces a $k$-body interaction with a chain of 2-body interactions through auxiliary "mediator" qubits, such that the low-energy effective Hamiltonian reproduces the original $k$-body term. More structured than the general [[Perturbation Gadgets for Locality Reduction|perturbation gadget]] approach.

## The trick

To simulate a $k$-body term $H_{1\ldots k}$ acting on qubits $q_1, \ldots, q_k$:

1. Introduce $k-2$ mediator qubits $m_1, \ldots, m_{k-2}$
2. Connect them in a chain: $q_1 - m_1 - m_2 - \cdots - m_{k-2} - q_k$, with 2-body interactions along each link
3. Add a large penalty $\Delta \gg \|H_{1\ldots k}\|$ for the mediators being in "wrong" states
4. In the low-energy subspace (mediators in their ground state), second-order perturbation theory yields an effective $k$-body interaction matching $H_{1\ldots k}$

The effective Hamiltonian is:
$$
H_{\mathrm{eff}} = H_{1\ldots k} + O(1/\Delta)
$$

## Why it works

The mediator qubits create a "virtual" pathway for the $k$-body interaction. At energy scales below $\Delta$, the mediators are frozen in their ground state and can be traced out, leaving the desired $k$-body effective interaction. This is analogous to how massive particles mediate effective interactions in quantum field theory.

## When to reach for it

- QMA-completeness reductions (the original use: 5-local → 2-local)
- Constructing 2-local Hamiltonians for adiabatic computation ([[Snake-Path 2D Embedding for Nearest-Neighbor Universality|2D nearest-neighbor]] constructions)
- Any setting where you need to implement a multi-body interaction using only pairwise couplings

## Caveat

- Introduces auxiliary qubits (system size grows)
- The spectral gap of the gadget Hamiltonian is $O(\Delta)$, but the effective gap within the computational subspace can shrink polynomially
- Only valid perturbatively: $\Delta$ must be much larger than the energy scale of $H_{1\ldots k}$
- Higher-order corrections in $1/\Delta$ must be bounded

## Related notes

- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[Perturbation Gadgets for Locality Reduction]]
- [[Perturbation Lemma for Locality Reduction]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[Kitaev Geometric Lemma for Ground Space Angles]]
