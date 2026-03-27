---
aliases:
  - Product Formula (Trotter-Suzuki)
  - Trotter-Suzuki
---

**Concept hub** — 86 wikilink refs across the vault.

---

## What it is

[[Product Formulas]] (Trotter–Suzuki formula) approximates the time evolution $e^{-iHt}$ for $H = \sum_j H_j$ by composing the individual evolutions $e^{-iH_j t}$:

$$e^{-iHt} \approx \left(\prod_j e^{-iH_j t/r}\right)^r$$

Higher-order formulas achieve better accuracy by symmetrising and nesting the decomposition (Suzuki's recursive construction). Error depends on commutators $[H_j, H_k]$, not just the norm of $H$.

---

## Key papers in this vault

### Foundations and error analysis
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2011]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2010]] — ordered exponential extensions (Suzuki recursion)
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu 2019]] — tight commutator-scaling bounds; best general error analysis
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs & Wiebe 2013]]

### Randomized [[Product Formulas]]
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs, Ostrander & Su 2019]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell 2019 (qDRIFT)]]
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes|Cho, Berry & Hsieh 2022]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann, Steudtner, Kueng, Kieferová & Eisert 2022]]

### Corrected [[Product Formulas]]
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab et al. 2024]]
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. 2025]]

### Chemistry applications
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. 2015]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran, Su, Childs & Wiebe 2021]]

---

## See also

- [[Hamiltonian simulation]] — broader context
- [[Hamiltonian Simulation — Comparison Tables]] — cost comparison
