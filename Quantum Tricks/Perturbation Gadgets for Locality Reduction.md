
> **Source:** Kempe, Kitaev & Regev, arXiv:quant-ph/0406180, SICOMP 35(5), 2006
> **Tags:** #trick #perturbation #locality #QMA #2-local

## What it does

Reduces $k$-local Hamiltonian interactions to 2-local using auxiliary "mediator" qubits. The low-energy effective Hamiltonian of the gadget reproduces the target $k$-local term via perturbation theory. More refined than the simple [[Perturbation Lemma for Locality Reduction|energy-penalty approach]].

## The trick

To simulate a $k$-body interaction $H_{\mathrm{target}}$ acting on qubits $q_1, \ldots, q_k$:

1. Introduce auxiliary qubits (mediators) that interact pairwise with the original qubits
2. Add a large energy penalty $\Delta$ for the mediators being in wrong states
3. The effective Hamiltonian in the low-energy subspace (via second-order perturbation theory) reproduces $H_{\mathrm{target}}$ up to error $O(1/\Delta)$

The construction yields a 2-local Hamiltonian whose ground energy approximates the ground energy of the original $k$-local Hamiltonian within $1/\mathrm{poly}(n)$.

## When to reach for it

- QMA-completeness reductions (the original use)
- Constructing physically realistic Hamiltonians from abstract $k$-local specifications
- Adiabatic computation with 2-local nearest-neighbor interactions (as in [[Snake-Path 2D Embedding for Nearest-Neighbor Universality|Aharonov et al. 2004]])

## Caveat

The spectral gap of the gadget Hamiltonian shrinks polynomially with $\Delta$. The gadget is perturbative — only valid when $\Delta$ is much larger than the energy scale of $H_{\mathrm{target}}$. Also introduces auxiliary qubits, increasing the total system size.

## Related notes

- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]] — uses gadgets systematically for the full dichotomy classification
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]] — uses gadgets to construct universal Hamiltonians
- [[Perturbation Lemma for Locality Reduction]] — the simpler penalty-based approach
- [[Mediator Qubit Gadget for Interaction Generation]] — a second-order variant for generating new interactions
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
