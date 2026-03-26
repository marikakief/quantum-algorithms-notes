# Analytical Gradient Estimation for Variational Quantum Circuits

> **Source:** Romero, Babbush, McClean, Hempel, Love, Aspuru-Guzik, arXiv:1701.02691 (2018)
> **Tags:** #trick #VQE #gradient #variational #Hadamard-test #optimization

## What it does

Computes exact energy gradients $\partial E / \partial t_j$ for a product-of-Pauli-exponentials ansatz using a Hadamard test circuit. Reduces measurement overhead for gradient estimation by orders of magnitude compared to numerical finite differences.

## The trick

For an ansatz of the form

$$U(\vec{t}) = \prod_j \prod_{k=1}^{N_j^S} e^{i c_{jk} t_j P_{jk}}$$

where $P_{jk}$ are Pauli strings and $c_{jk}$ are constants from the fermion-to-qubit mapping, the exact gradient of $E = \langle H \rangle$ with respect to parameter $t_j$ is:

$$\frac{\partial E}{\partial t_j} = 2\sum_i h_i \sum_k c_{jk}\, \text{Im}\!\left\langle \Phi_0 \middle| V_{jk}^\dagger(\vec{t})\, O_i\, U(\vec{t}) \middle| \Phi_0 \right\rangle$$

where $V_{jk}(\vec{t})$ is the same product ansatz but with factor $P_{jk}$ inserted between the $(jk)$-th and $(jk+1)$-th exponentials.

**Circuit for each imaginary overlap** (Hadamard test):
1. Ancilla qubit: prepare $(|0\rangle + |1\rangle)/\sqrt{2}$ via Hadamard.
2. Apply $U(\vec{t})$ up to position $(jk)$ on the state register (controlled on ancilla = 1 or unconditionally — depends on decomposition).
3. Apply controlled-$P_{jk}$ on state register (ancilla controls).
4. Continue applying remaining exponentials.
5. Apply controlled-$O_i$ on state register.
6. Hadamard on ancilla, measure in $Y$ basis ($\langle \sigma_y \rangle$ gives the imaginary part).

Each $\langle \sigma_y \rangle$ measurement contributes one term to the gradient sum.

**Sampling cost for gradient of parameter $j$ to precision $\tilde{\varepsilon}_j$:**

$$\tilde{m}_j \leq \frac{4\left(\sum_i |h_i|\right)^2}{\tilde{\varepsilon}_j^2}$$

**Comparison — numerical central difference (step $\delta$):**

$$\tilde{m}_j^\text{num} \approx \frac{4\left(\sum_i |h_i|\right)^2}{(2\delta\,\tilde{\varepsilon}_j)^2} = \frac{\tilde{m}_j}{(2\delta)^2}$$

So analytical gradient needs $(2\delta)^2$ times *fewer* measurements than numerical gradient. For $\delta = 0.05$ (a typical finite-difference step size), this is a factor of $\sim 100$.

## When to reach for it

- Whenever you need gradients for a [[Variational Quantum Eigensolver (VQE)|VQE]] optimization with many parameters ($\gtrsim 10$) and measurement budget is the bottleneck.
- When using gradient-based classical optimizers (L-BFGS-B, Adam, etc.) as opposed to derivative-free methods (Nelder-Mead, COBYLA).
- In hardware experiments where re-preparation of the state is cheap but shot noise is the dominant error source.
- As a building block in more advanced variational algorithms that require gradient information (e.g., quantum natural gradient).

## Complexity

- **Measurement cost:** $O(N_P \cdot N^4 / \tilde{\varepsilon}^2)$ total, where $N_P$ is the number of parameters and $N^4$ is the number of Pauli terms in $H$. Factor of $\sim 100$–$10^4$ saving over numerical gradients for typical $\delta$.
- **Circuit overhead:** Each gradient term requires a Hadamard test with one ancilla qubit. The circuit depth is comparable to the ansatz depth itself (one forward pass through $U$, roughly).
- **Parameter shift rule:** A related (and in some ways simpler) approach exists for Pauli generators with eigenvalues $\pm 1/2$: $\partial E/\partial t_j = \frac{1}{2}[E(t_j + \pi/2) - E(t_j - \pi/2)]$. No ancilla required; each gradient element needs two energy evaluations. The parameter shift rule is now the standard approach on hardware, largely superseding the Hadamard test version in this paper.

## Caveat

- **Hadamard test requires controlled unitaries:** Each gradient term needs a controlled version of part of $U$ — expensive on hardware with limited connectivity or no native controlled-$n$-qubit gates. The parameter shift rule avoids this at the cost of $2N_P$ energy evaluations per gradient step.
- **Does not reduce energy measurement cost:** The $(\sum|h_i|)^2/\varepsilon^2$ sampling cost for *energy* estimation is unchanged. Analytical gradients only help with *gradient* estimation.
- **Overhead per gradient element:** The number of circuits to run per gradient step is $O(N_P \times N^4)$, scaling with both parameter count and Hamiltonian terms. Can be parallelized across measurement settings.
- **Barren plateaus:** If the ansatz exhibits barren plateaus (exponentially vanishing gradients), analytical gradients will be exponentially small in system size — you're estimating zero very precisely. Prescreening and good initialization ([[MP2-Based Amplitude Prescreening for UCC]]) help avoid barren plateaus.

## Related notes
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Pauli Expectation Value Estimation]]
- [[Probability Oracle from Hadamard Test]] — Same Hadamard test circuit repurposed as a probability oracle for the quantum gradient algorithm
- [[Gradient Encoding for Multi-Observable Estimation]] — Uses the Hadamard test to encode expectation values (not variational gradients) as a function gradient
