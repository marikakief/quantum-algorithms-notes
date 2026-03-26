

> **Tags:** #trick #ancilla-design #circuit-construction #select-oracle
> **Source:** Berry, Childs, Cleve, Kothari, Somma. arXiv:1412.4687

## What it does
Encodes a variable-length product of unitaries $H_{\ell_1} \cdots H_{\ell_k}$ as a fixed-depth circuit using unary encoding of $k$, avoiding explicit enumeration of $L^k$ terms.

## The trick
Encode the Taylor order $k$ in **unary**: $|k\rangle = |1^k 0^{K-k}\rangle$ (first $k$ qubits are $|1\rangle$, rest $|0\rangle$).

Give each position $\kappa = 1, \ldots, K$ its own index register $|\ell_\kappa\rangle$.

The [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] oracle is then $K$ sequential **controlled-[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]](H)** operations:
$$\text{c-[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]}_\kappa: |b\rangle|\ell\rangle|\psi\rangle \to |b\rangle|\ell\rangle(-iH_\ell)^b|\psi\rangle$$

where $b$ is the $\kappa$-th qubit of the unary register. If $b = 0$ (meaning $\kappa > k$), nothing happens. If $b = 1$, apply $-iH_\ell$.

The product $(-i)^k H_{\ell_1} \cdots H_{\ell_k}$ emerges automatically from the sequential controlled operations.

## When to reach for it
- Implementing products of variable numbers of operators (Taylor series, path integrals)
- When the number of terms in each product is bounded by $K$ but varies
- Avoiding the exponential blowup of explicitly enumerating all $L^k$ terms

## Complexity
$K$ sequential controlled-[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] operations, each costing $O(L(n + \log L))$ for Pauli-product Hamiltonians. Total: $O(KL(n + \log L))$.

## Caveat
Ancilla cost is $O(K \log L)$ qubits — one $\log L$-qubit register per Taylor order position. The [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] oracle must create correlated superpositions across all $K+1$ registers.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Unary Iteration]] — closely related later primitive from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]]. This card is earlier and specific to unary-encoding the Taylor truncation order; [[Unary Iteration]] generalizes the "sweep a unary control" idea to binary-indexed table lookup and SELECT gadgets.
