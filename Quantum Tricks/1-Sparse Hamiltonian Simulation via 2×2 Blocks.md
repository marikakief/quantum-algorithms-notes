
> **Source:** Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139 (2005)
> **Tags:** #trick #sparse #simulation #base-case

## What it does

Simulates time evolution under a 1-sparse Hermitian matrix exactly (up to oracle cost), by decomposing it into independent $2 \times 2$ and $1 \times 1$ blocks.

## The trick

A 1-sparse Hermitian matrix has at most one off-diagonal nonzero per row/column. Its structure is a union of:

- **Isolated vertices:** diagonal entry $H_{xx}$. Evolution is a phase gate $e^{-iH_{xx}t}$.
- **Matched pairs $(x, y)$:** a $2 \times 2$ block $\begin{pmatrix} H_{xx} & H_{xy} \\ H_{yx} & H_{yy} \end{pmatrix}$.

Each $2 \times 2$ block is diagonalized analytically. The eigenvalues are

$$
\lambda_\pm = \frac{H_{xx} + H_{yy}}{2} \pm \sqrt{\left(\frac{H_{xx} - H_{yy}}{2}\right)^2 + |H_{xy}|^2}.
$$

Implementation: one oracle query to find the neighbor $y$ and read off $H_{xx}, H_{xy}, H_{yy}$. Then a controlled rotation on the $\{|x\rangle, |y\rangle\}$ subspace applies $e^{-iH_{\text{block}}t}$.

## Why it matters

This is the base case for all [[Order-Condition Cancellation in Product Formulas|product-formula]] sparse simulation. Every higher-level scheme ([[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Trotter]], [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki]], [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|randomized formulas]]) ultimately calls this as the primitive for simulating individual terms.

## Complexity

- **Queries:** 1 call to the black-box per block
- **Gates:** $O(1)$ per block (controlled rotation + phase)
- **Total for one 1-sparse term on $n$ qubits:** $O(1)$ oracle calls (the decomposition is implicit — you only touch the block containing your input state)

## Caveat

Only works for 1-sparse matrices. Going from $d$-sparse to 1-sparse requires a decomposition step (e.g., [[Edge-Coloring Decomposition for Sparse Hamiltonians]]) that introduces overhead.

## Related notes

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]]
- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
