# Mediator Qubit Gadget for Interaction Generation

> **Source:** Cubitt & Montanaro, arXiv:1311.3161; Oliveira & Terhal, arXiv:quant-ph/0504050
> **Tags:** #trick #perturbation #gadget #QMA #2-local

## What it does

Generates an effective 2-qubit interaction between qubits $a$ and $c$ that wasn't present in the original interaction set, by routing through a mediator qubit $b$ with a strong energy penalty.

## The trick

Set up:
- Qubits $a$, $b$, $c$ with interactions $H^{(1)}_{ab}$ and $H^{(2)}_{bc}$ from the available set $S$
- Strong penalty $\Delta |\psi\rangle\langle\psi|_b$ on the mediator qubit, with $\Delta \gg \|H^{(1)}\|, \|H^{(2)}\|$

The overall Hamiltonian is $\tilde{H} = \Delta |\psi\rangle\langle\psi|_b + \sqrt{\Delta}\, H^{(1)}_{ab} + \sqrt{\Delta}\, H^{(2)}_{bc} + L_a + M_c$, where $L_a, M_c$ are 1-local correction terms.

In the low-energy subspace (mediator projected to $|\psi^\perp\rangle$), the first-order contribution from $H^{(1)}, H^{(2)}$ vanishes. The second-order term gives:
$$
H_{\mathrm{eff}} \approx -2 \sum_{i,j} \mathrm{Re}\!\bigl(\langle \psi^\perp | B^{(i)} | \psi \rangle \langle \psi | C^{(j)} | \psi^\perp \rangle\bigr)\, A^{(i)}_a \otimes D^{(j)}_c
$$
where $H^{(1)} = \sum_i A^{(i)} \otimes B^{(i)}$ and $H^{(2)} = \sum_j C^{(j)} \otimes D^{(j)}$.

The key flexibility: **by choosing different penalty states $|\psi\rangle$**, the same pair of interactions produces different effective cross-terms. For example:
- $|\psi\rangle = |1\rangle$ with $H = XX + \gamma ZZ$: effective interaction $\propto XX$
- $|\psi\rangle = |-\rangle$ with $H = XX + \gamma ZZ$: effective interaction $\propto ZZ$
- $|\psi\rangle = |+\rangle$ with $H = XZ - ZX$: effective interaction $\propto ZZ$

## When to reach for it

- Complexity classification of local Hamiltonian problems (producing interactions you don't have from interactions you do)
- Building [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes|universal Hamiltonians]] via simulation chains
- Any QMA-hardness reduction where you need to expand the interaction set

## Complexity

Adds 1 auxiliary qubit per use. Can be applied in parallel across different mediator qubits (the gadgets don't interfere because each acts on distinct mediator qubits). Can only be applied in series a constant number of times, since each use increases the Hamiltonian norm by a polynomial factor.

## Caveat

- The $\sqrt{\Delta}$ scaling of the interaction terms is needed for the second-order term to be $O(1)$. Higher-order corrections are $O(1/\Delta)$.
- 1-local correction terms $L_a, M_c$ must be chosen to cancel unwanted first-order contributions. Without free access to 1-local terms, this gadget doesn't directly apply (the "without local terms" case uses different techniques).
- Error is $O(\delta^{-1})$ where $\Delta = \delta^2 \|H_{\mathrm{else}}\|^2$, so high precision requires large penalties.

## Related notes

- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
- [[Perturbation Gadgets for Locality Reduction]]
- [[Subdivision Gadget for k-to-2-Local Reduction]]
- [[Perturbation Lemma for Locality Reduction]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
