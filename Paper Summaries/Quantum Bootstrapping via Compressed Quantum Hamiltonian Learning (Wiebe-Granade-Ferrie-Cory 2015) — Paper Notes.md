# Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015)

> **🚧 STUB** — referenced from Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2014) notes.

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Nathan Wiebe, Christopher Granade, Christopher Ferrie, D.G. Cory |
| **Year** | 2015 |
| **Venue** | *New Journal of Physics* **17**, 022005 (2015) |
| **arXiv** | [arXiv:1409.1524](https://arxiv.org/abs/1409.1524) |
| **Tags** | #Hamiltonian-learning #compressed-sensing #bootstrapping #parameter-estimation |

---

## Key result

Extends quantum Hamiltonian learning (QHL) to the *sparse* unknown Hamiltonian setting using compressed sensing. Rather than assuming the functional form of $H$ is known (as in the base QHL paper), bootstrapping identifies which terms are present by sampling a compressed sensing measurement matrix and solving a sparse recovery problem.

The "bootstrapping" refers to using a coarse estimate of $H$ (from fewer measurements) to design better experiments for refining the estimate — an adaptive strategy that reduces total resource cost.

---

## Relation to other Hamiltonian learning papers

- Extends [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory 2014 (QHL)]] — that paper assumes the model family is known; this one discovers it
- Builds on [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes|Granade-Ferrie-Wiebe-Cory 2012 (Robust Online HL)]] — the SMC/Bayesian update framework

---

## TODO

- [ ] Full description of the compressed sensing measurement design
- [ ] Sample complexity bounds and comparison with QHL
- [ ] Relation to structure learning in quantum systems
