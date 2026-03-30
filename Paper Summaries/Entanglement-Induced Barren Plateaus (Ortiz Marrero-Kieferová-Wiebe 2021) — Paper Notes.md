# Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021)

> **🚧 STUB** — needs full treatment. Marika is an author; this paper deserves thorough notes.

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Carlos Ortiz Marrero, Mariia Kieferová, Nathan Wiebe |
| **Year** | 2021 |
| **Venue** | *PRX Quantum* **2**, 040316 (2021) |
| **arXiv** | [arXiv:2007.07868](https://arxiv.org/abs/2007.07868) |
| **Citations** | ~467 (as of early 2026) |
| **Tags** | #barren-plateaus #VQA #entanglement #trainability |

---

## Key result

High entanglement across a subsystem boundary in a variational ansatz causes exponentially vanishing gradients — *barren plateaus* — regardless of the specific circuit structure. Specifically: if the reduced state on a subsystem is close to maximally mixed (as measured by purity), all gradients with respect to parameters in that subsystem vanish exponentially in subsystem size.

This gives a *structural* explanation for barren plateaus that goes beyond the earlier "random circuit" argument of McClean et al. (2018) — the mechanism is entanglement, not randomness.

---

## Why it matters

- Connects barren plateau trainability to a physically meaningful quantity (entanglement/purity) rather than just circuit depth or randomness
- Implies that ansätze preserving low entanglement (e.g. tensor-network-inspired, symmetry-constrained) are trainability-friendly
- Has influenced subsequent work on barren plateau diagnostics and mitigation strategies

---

## Connection to Jozsa-Linden

[[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden (2003)]] proved that exponential quantum speedup on pure states *requires* large multi-partite entanglement. This paper shows the flip side: *too much* entanglement across a subsystem boundary causes barren plateaus, killing gradient signal exponentially. Together, the two results bracket the useful entanglement regime for variational quantum algorithms — you need enough entanglement for computational power, but not so much that gradients vanish.

---

## TODO

- [ ] Full derivation of the purity-gradient bound
- [ ] Comparison with noise-induced barren plateaus (Wang et al. 2021)
- [ ] Links to subsequent mitigation strategies in the vault
- [ ] Relation to [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Kieferová's other work on Trotter]]
- [ ] Relation to [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes|Kieferová-Wiebe (2017)]] — earlier QBM work by the same authors; the relative entropy gradient used there doesn't obviously suffer from barren plateaus (signal from expectation differences, not random unitaries), but the question is open for deep QBMs
