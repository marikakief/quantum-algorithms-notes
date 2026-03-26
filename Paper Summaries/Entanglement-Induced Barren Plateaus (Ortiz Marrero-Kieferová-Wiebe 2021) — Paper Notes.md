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

## TODO

- [ ] Full derivation of the purity-gradient bound
- [ ] Comparison with noise-induced barren plateaus (Wang et al. 2021)
- [ ] Links to subsequent mitigation strategies in the vault
- [ ] Relation to [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Kieferová's other work on Trotter]]
