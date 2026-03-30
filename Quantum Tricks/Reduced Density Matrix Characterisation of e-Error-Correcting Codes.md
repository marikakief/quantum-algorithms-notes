# Reduced Density Matrix Characterisation of e-Error-Correcting Codes

> **Source:** E. Knill & R. Laflamme, arXiv:quant-ph/9604034
> **Tags:** #trick #QEC #error-correction #reduced-density-matrix #codes

## What it does
Characterises $e$-error-correcting quantum codes in terms of reduced density matrices: all codewords must look identical on any $2e$ qubits and be distinguishable on the complementary qubits.

## The trick
Let $C$ be a code on $r$ qubits with orthonormal basis $\{|i_L\rangle\}$. For a subset $U \subseteq \{1, \ldots, r\}$, let $\rho(|x\rangle, U)$ denote the reduced density matrix of $|x\rangle$ on the qubits in $U$.

**Theorem (Knill-Laflamme).** $C$ is $e$-error-correcting if and only if for all $U$ with $|U| = 2e$:

1. $\rho(|i_L\rangle, U) = \rho(|j_L\rangle, U)$ for all $i, j$ (all codewords have identical reduced states on any $2e$ qubits)
2. $\rho(|i_L\rangle, \bar{U}) \cdot \rho(|j_L\rangle, \bar{U}) = 0$ for $i \neq j$ (different codewords are distinguishable on the complementary $r - 2e$ qubits)

**Intuition:** The code "hides" the encoded information from any $2e$ qubits. An adversary who corrupts at most $e$ qubits and examines at most $e$ qubits of the syndrome learns nothing about which codeword was encoded. The complementary qubits retain enough information to distinguish codewords and enable recovery.

This is the quantum analogue of the classical property that an $e$-error-correcting code has minimum Hamming distance $\geq 2e + 1$: no $2e$ coordinates can distinguish between codewords.

**Application to the quantum Hamming bound:** The condition that all codewords have identical reduced density matrices on $2e$ qubits, combined with the orthogonality condition on the complement, constrains the code parameters. For a $(2^r, k)$ code: $r \geq 4e + \lceil\log k\rceil$.

The proof that 4 qubits can't support a 1-error-correcting qubit code follows immediately: with $r = 4$ and $e = 1$, condition (1) forces $\rho(|0_L\rangle, U) = \rho(|1_L\rangle, U)$ on any 2 qubits, while condition (2) forces $\rho(|0_L\rangle, \bar{U}) \cdot \rho(|1_L\rangle, \bar{U}) = 0$ on the other 2 qubits. But $\bar{U}$ also has only 2 qubits, so the reduced density matrices on $\bar{U}$ must be both equal (by applying condition 1 to $\bar{U}$) and orthogonal — contradiction.

## When to reach for it
- Verifying that a proposed code is $e$-error-correcting without computing all $\langle i_L | A_a^\dagger A_b | j_L \rangle$ inner products — just check reduced density matrices.
- Proving impossibility results (quantum Hamming bounds, minimum qubit counts).
- Understanding the information-theoretic content of quantum codes: the code distributes information so that any small subset of qubits reveals nothing about the encoded state.
- Connections to quantum secret sharing: an $e$-error-correcting code is automatically a quantum secret-sharing scheme where any $2e$ qubits form an "unauthorised set".

## Complexity
Computing reduced density matrices on $2e$ qubits requires tracing out $r - 2e$ qubits from each codeword: $O(2^{2(r-2e)} \cdot 2^{4e})$ operations per check. Checking all $\binom{r}{2e}$ subsets scales polynomially in $r$ for fixed $e$.

## Caveat
This characterisation is for codes correcting independent $e$-qubit errors specifically. For general (correlated) error models, you need the full [[Knill-Laflamme Conditions for Quantum Error Correction|Knill-Laflamme conditions]] with the specific error operators $\{A_a\}$. The reduced density matrix version doesn't extend directly to approximate QEC — the exact equality conditions don't have a clean approximate analogue in terms of reduced states alone.

## Related notes
- [[Theory of Quantum Error-Correcting Codes (Knill-Laflamme 1997) — Paper Notes]]
- [[Knill-Laflamme Conditions for Quantum Error Correction]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
