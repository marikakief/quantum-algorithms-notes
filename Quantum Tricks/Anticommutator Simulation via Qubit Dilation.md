
> **Source:** Childs & Wiebe, arXiv:1211.4945, J. Math. Phys. 54, 062202 (2013)
> **Tags:** #trick #hamiltonian-simulation #dilation #many-body #product-formula

## What it does

Converts simulation of a many-body interaction term (a product of commuting operators $A_1^{\alpha_1} \cdots A_m^{\alpha_m}$) into simulation of a nested commutator, so that the [[Recursive Group-Commutator Product Formula]] machinery applies. The conversion uses a simple qubit dilation: embed each operator in $H \otimes \mathbb{C}^2$.

## The trick

Define the dilated operators
$$
\tilde{A}_j = A_j \otimes \sigma_z, \qquad \tilde{B}_j = I \otimes \sigma_x,
$$
acting on $\mathcal{H} \otimes \mathbb{C}^2$ for each $j$. Their commutator is:
$$
[\tilde{A}_j, \tilde{B}_j] = A_j \otimes [\sigma_z, \sigma_x] = 2i A_j \otimes \sigma_y.
$$
Repeating this construction and taking iterated commutators, one shows that for commuting Hermitian operators $A_1, \ldots, A_m$ with multiplicities $\alpha_\ell$, the total nesting depth $k = \sum_\ell \alpha_\ell - 1$ yields a nested commutator proportional to $A_1^{\alpha_1} \cdots A_m^{\alpha_m} \otimes P$ for some known Pauli $P$ on the ancilla register.

The result (Theorem 13): $\exp(-i \, 2^{-k} A_1^{\alpha_1} \cdots A_m^{\alpha_m} t^{k+1})$ (on the target system) can be implemented using the nested commutator [[product formula]] $U_p$ on the dilated system, with cost $O(6^{pk}(6^{pk} \Lambda t)^{k+1+(k+1)^2/(2p)} \varepsilon^{-(k+1)/(2p)})$ elementary exponentials.

## When to reach for it

- Simulating $k$-local Hamiltonians where interaction terms are products of single-site operators that commute pairwise — common in condensed matter and chemistry models.
- Whenever you want to implement $e^{i c A_1 A_2 \cdots A_k t}$ and each $e^{iA_j t'}$ is efficiently realizable.
- Quantum control: generating arbitrary unitaries via repeated commutator evolution.

## Complexity

The overhead is $2(k+1)$ ancilla qubits total (one qubit per dilated operator, across the nesting levels). Gate cost matches Theorem 13 above — scales as $(\Lambda t)^{k+1+o(1)}$.

## Caveat

Only works for pairwise commuting $A_j$. If the operators do not commute, the construction does not reproduce the desired product and a different approach is needed. The nesting depth $k = \sum_\ell \alpha_\ell - 1$ grows with the interaction order, so high-body terms (large $k$) are expensive.

## Related notes

- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Recursive Group-Commutator Product Formula]]
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]]
