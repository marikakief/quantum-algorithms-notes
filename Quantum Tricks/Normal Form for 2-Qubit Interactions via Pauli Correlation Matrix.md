# Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix

> **Source:** Cubitt & Montanaro, arXiv:1311.3161, SICOMP 45(2), 2016
> **Tags:** #trick #normal-form #local-unitary #classification

## What it does

Reduces any traceless 2-qubit Hermitian matrix to a canonical form using only local-unitary conjugation $U^{\otimes 2}$, making it possible to classify all 2-qubit interactions by a small number of normal-form cases.

## The trick

Any traceless 2-qubit Hermitian $H$ decomposes as:
$$
H = \sum_{i,j=1}^{3} M_{ij}\, \sigma_i \otimes \sigma_j + \sum_{k=1}^{3} v_k \sigma_k \otimes I + w_k I \otimes \sigma_k
$$
where $M(H)$ is the $3 \times 3$ **Pauli correlation matrix**. Conjugation by $U^{\otimes 2}$ acts as an $\mathrm{SO}(3)$ rotation on $M$: $M \mapsto R M R^T$ for $R \in \mathrm{SO}(3)$ corresponding to $U$.

For **symmetric** $H$ (under qubit exchange), $M$ is symmetric, so can be diagonalised by $R$:
$$
U^{\otimes 2} H (U^\dagger)^{\otimes 2} = \alpha\, XX + \beta\, YY + \gamma\, ZZ + (\text{1-local terms})
$$

For **antisymmetric** $H$, $M$ is skew-symmetric, and the normal form reduces to $\alpha(XZ - ZX)$ plus 1-local terms.

Any 2-qubit interaction can be split into symmetric + antisymmetric parts (using the swap operator), so these two cases cover everything.

## When to reach for it

- Classifying the complexity of local Hamiltonian problems (the original use)
- Reducing the number of cases in any argument that needs to handle "all possible 2-qubit interactions"
- Checking whether a set of interactions is simultaneously locally diagonalisable (relevant for stoquasticity)

## Complexity

$O(1)$ — the rotation $R$ is found by diagonalising a fixed-size $3 \times 3$ matrix.

## Caveat

The same $U$ must be applied to both qubits (it's $U^{\otimes 2}$, not $U_1 \otimes U_2$). This is necessary to preserve the eigenvalues of many-body Hamiltonians built from these interactions, but it's more restrictive than the general two-qubit normal form (which allows different unitaries on each qubit).

## Related notes

- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[Perturbation Gadgets for Locality Reduction]]
