# Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012)

> **🚧 STUB** — referenced from Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2014) notes.

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Christopher Granade, Christopher Ferrie, Nathan Wiebe, D.G. Cory |
| **Year** | 2012 |
| **Venue** | *New Journal of Physics* **14**, 103013 (2012) |
| **arXiv** | [arXiv:1207.1655](https://arxiv.org/abs/1207.1655) |
| **Tags** | #Hamiltonian-learning #Bayesian #sequential-Monte-Carlo #parameter-estimation |

---

## Key result

Introduces sequential Monte Carlo (SMC) methods for online Bayesian Hamiltonian parameter estimation. Given measurement outcomes from experiments on an unknown quantum system, maintain a particle-filter approximation to the posterior distribution over Hamiltonian parameters and update it after each experiment.

Key features:
- **Online**: processes measurements sequentially, no batch re-processing
- **Robust**: handles finite sampling noise and approximate models via Liu-West resampling
- **Adaptive**: can choose experiments (evolution times) to maximise information gain at each step

Precursor to the full QHL framework of Wiebe-Granade-Ferrie-Cory 2014, which adds quantum simulation resources to the experiment design.

---

## Relation to other Hamiltonian learning papers

- Direct precursor to [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory 2014 (QHL)]]
- The SMC + Bayesian update framework is inherited by [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes|bootstrapping paper]]

---

## TODO

- [ ] Full SMC algorithm description and convergence guarantees
- [ ] Comparison of classical vs quantum resources for experiment design
- [ ] Liu-West resampling details
