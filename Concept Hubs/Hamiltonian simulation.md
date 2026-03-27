# Hamiltonian Simulation

**Concept hub** — 128 wikilink refs across the vault.

---

## What it is

The task of simulating the time evolution $e^{-iHt}$ of a quantum system with Hamiltonian $H$ on a quantum computer. The core problem in quantum algorithms for physics and chemistry: given a description of $H$ and a time $t$, produce a unitary (or approximate it to precision $\varepsilon$) that acts like $e^{-iHt}$.

Complexity is measured in terms of $t$, $\varepsilon$, the system size $n$, and a "sparsity" or norm parameter characterising $H$.

---

## Main algorithmic approaches in this vault

### Product formulas (Trotter–Suzuki)
Decompose $H = \sum_j H_j$ and approximate $e^{-iHt}$ by alternating simpler exponentials.

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders 2005]] — early sparse simulation
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2011]] — Trotter for chemistry
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2010]] — higher-order ordered exponentials
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu 2019]] — tight commutator-scaling error bounds
- [[Product Formulas|Product formula]] (concept page)

### Taylor series / LCU
Truncate the Dyson/Taylor series and implement via linear combination of unitaries.

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry, Childs, Cleve, Kothari & Somma 2015]]
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry, Childs, Cleve, Kothari & Somma 2014]]

### Qubitization
Encode $H/\lambda$ as a block of a unitary and use quantum walk / eigenvalue transformation.

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang 2019]]

### Quantum Signal Processing / QSVT
Apply polynomial transformations to block-encoded matrices; unifies and improves essentially all prior methods.

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang 2016/2017]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe 2018/2019]]

### Interaction picture
Move part of $H$ into the interaction picture to reduce effective norm.

- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe 2018]]

---

## See also

- [[Hamiltonian Simulation — Comparison Tables]] — cost comparison across methods
- [[Product Formulas]]
- [[Amplitude Amplification and Estimation]]
