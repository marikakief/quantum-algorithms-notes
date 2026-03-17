> **Source:** Julia Kempe and Oded Regev, *3-local Hamiltonian is QMA-complete*, Quantum Inf. Comput. **3**(3), 258–264 (2003)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0302079) · [QIC](https://doi.org/10.26421/QIC3.3-7)
> **Tags:** #QMA #local-hamiltonian #locality-reduction #perturbation

---

## What the paper does

Reduces the locality of the QMA-complete local Hamiltonian problem from 5 (Kitaev) to 3. The key technique — [[Perturbation Lemma for Locality Reduction|adding a large energy penalty $J$ for illegal states]] and bounding the spectral perturbation — was later reused extensively in the [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|adiabatic equivalence paper]] for the 5→3 local reduction of the adiabatic Hamiltonian.

---

## The reduction

Replace each 5-local propagation term $H_\ell$ (which involves 3 clock qubits + 2 computation qubits) with a 3-local version $H'_\ell$ that uses only 1 clock qubit + 2 computation qubits. The simplified term no longer preserves the legal clock subspace, so amplify the clock penalty: $J \cdot H_{\mathrm{clock}}$ with $J$ polynomially large.

The [[Perturbation Lemma for Locality Reduction|perturbation lemma]] (Lemma 3.17 in Aharonov et al. 2004, originating here) shows the low-energy spectrum shifts by at most $O(K^2/(J-2K))$, which is negligible when $J \gg K^2/\text{gap}$.

---

## Reusable ideas

1. **[[Perturbation Lemma for Locality Reduction]]:** The technique of pushing illegal states to high energy and controlling the spectral perturbation. Originated here, became a standard gadget for locality reduction throughout Hamiltonian complexity and adiabatic computation.

---

## References within this paper

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999/2002)]] — 5-local QMA-completeness

---

## Cross-links

### Paper notes
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] — the 5-local result this improves
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]] — further reduces to 2-local
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — uses the perturbation technique from this paper

### Trick cards
- [[Perturbation Lemma for Locality Reduction]]
- [[History-State Encoding with Unary Clock]]
