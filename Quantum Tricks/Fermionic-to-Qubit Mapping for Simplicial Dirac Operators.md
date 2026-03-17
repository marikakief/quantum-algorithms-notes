
> **Tags:** #trick #jordan-wigner #block-encoding #tda
> **Source:** Babbush et al. arXiv:2209.13581, in the TDA block-encoding / Dirac-operator construction context

## What it does
Maps combinatorial Dirac operators on simplicial complexes into qubit Pauli operators so they can be block-encoded and processed by [[Qubitization Iterate|qubitization]]/[[QSVT Meta-Template|QSVT]].

## The trick
- Treat simplex-graded chain spaces as fermionic occupation sectors
- Write Dirac operator $D = \sum_k (\partial_k + \partial_k^\dagger)$ in fermionic form
- Apply Jordan–Wigner / Bravyi–Kitaev transform to express $D$ as Pauli strings
- Use [[Linear Combination of Unitaries (LCU)|LCU]]-style [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]/[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] to block-encode $D$

Because $D^2 = \Delta$ (Hodge Laplacian), once $D$ is encoded, both spectral and diffusion-like operations are available.

## When to reach for it
- Topological operators with creation/annihilation-like boundary maps
- Graded chain complexes where parity structure matters
- Need to move from abstract algebraic topology operators to concrete circuits

## Complexity
Depends on locality of boundary maps and encoding choice. JW has simple mapping but longer parity strings; BK reduces parity overhead with more complex logic.

## Caveat
Fermionic mapping overhead can dominate if operator terms are highly nonlocal in chosen ordering.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
