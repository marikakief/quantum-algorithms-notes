
> **Source:** Lloyd, Mohseni & Rebentrost, arXiv:1307.0401 (2014), Nature Physics 10, 631 (2014)
> **Tags:** #trick #density-matrix #Hamiltonian-simulation #non-sparse #quantum-ML

## What it does

Given $n$ copies of an unknown quantum state $\rho$, implements the unitary $e^{-i\rho t}$ on any other state $\sigma$. This turns **any density matrix into a Hamiltonian** — no sparsity or structural assumptions required.

## The trick

The partial swap on $\rho \otimes \sigma$:

$$
\mathrm{tr}_1\left[e^{-iS\Delta t}(\rho \otimes \sigma)e^{iS\Delta t}\right] = \sigma - i\Delta t[\rho, \sigma] + O(\Delta t^2)
$$

where $S = \sum_{ij}|ij\rangle\langle ji|$ is the swap operator. This is $e^{-i\rho\Delta t}\sigma\,e^{i\rho\Delta t}$ to first order in $\Delta t$.

**Procedure:**
1. Set $\Delta t = t/n$
2. For each of $n$ fresh copies of $\rho$: apply $e^{-iS\Delta t}$ to (copy, $\sigma$-register), then discard the copy
3. After $n$ steps: $\sigma \mapsto e^{-i\rho t}\sigma\,e^{i\rho t}$ up to error $\epsilon$

**Copy count:** $n = O(t^2/\epsilon)$ by Suzuki-Trotter analysis of the first-order [[Product Formulas]].

**Why $e^{-iS\Delta t}$ is efficient:** $S$ is a permutation matrix (sparse, 2-local in the sense of swapping two registers), so $e^{-iS\Delta t}$ can be implemented in $O(\log d)$ gates.

## When to reach for it

- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Quantum PCA]]: exponentiate $\rho$ to apply [[Gapped Phase Estimation|phase estimation]] and extract eigenvectors/eigenvalues
- Exponentiating **non-sparse** positive matrices: write $X = A^\dagger A$, prepare $\rho = X/\mathrm{tr}(X)$ from quantum-accessible columns, exponentiate via copies
- Any setting where you have multiple copies of a quantum state and want to use it as a Hamiltonian
- Quantum self-tomography: the state analyses itself

## Complexity

- Copies: $n = O(t^2/\epsilon)$
- Gates per copy: $O(\log d)$ for the partial swap
- Total: $O(t^2 \log d / \epsilon)$

For phase estimation to accuracy $\epsilon$: need $t = O(1/\epsilon)$, so copies = $O(1/\epsilon^3)$.

## Caveat

The $O(t^2/\epsilon)$ scaling is first-order Trotter. The paper argues this can't be improved to $O(t/\epsilon)$ — doing so would allow eigenvalue estimation to accuracy $O(1/n)$ from $n$ copies, violating the $O(1/\sqrt{n})$ statistical limit.

For **classical data** loaded via qRAM: Tang's dequantization results (2018) show classical algorithms achieve similar scaling with analogous data access. The genuine quantum advantage is for **quantum data** (actual copies of a quantum state), not classical data encoded into quantum states.

## Related notes

- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]]
- [[Quantum Self-Tomography via Phase Estimation]]
- [[Gapped Phase Estimation]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
