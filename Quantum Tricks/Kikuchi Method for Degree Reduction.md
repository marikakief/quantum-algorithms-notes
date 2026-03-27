
> **Source:** Wein, Alaoui, Moore, FOCS 2019; Hastings, Quantum 4, 237 (2020); Schmidhuber, O'Donnell, Kothari, Babbush, arXiv:2406.19378
> **Tags:** #trick #Kikuchi-method #degree-reduction #spectral-method #planted-inference #constraint-satisfaction

## What it does

Transforms a degree-$k$ polynomial optimization problem into a degree-2 problem (a graph eigenvalue problem) by lifting variables into a higher-dimensional space, enabling spectral methods.

## The trick

Given a $k$XOR instance on $n$ variables $x_1, \ldots, x_n \in \{\pm 1\}$, introduce $\binom{n}{\ell}$ new variables $y_T$ indexed by $\ell$-subsets $T \subset [n]$, with the intended meaning $y_T = \prod_{i \in T} x_i$.

Each original constraint $x_S = b$ (with $|S| = k$) generates equations $y_T y_U = b$ for all pairs $(T, U)$ with $|T| = |U| = \ell$ and $T \triangle U = S$. This produces the **Kikuchi graph** $\mathcal{K}_\ell$: an edge-signed graph on $\binom{n}{\ell}$ vertices with average degree:

$$d_{\mathcal{K}_\ell} = \Delta \cdot \binom{k}{k/2} \cdot \ell \cdot \left(\frac{\ell}{n}\right)^{(k-2)/2}$$

The key spectral property: for a planted assignment $z$, the lifted vector $z^{\odot \ell}$ achieves advantage $\rho$ on the Kikuchi graph, so $\lambda_{\max}(\mathcal{K}_\ell) \geq \rho \cdot d$. For random instances, matrix Chernoff bounds give $\lambda_{\max}(\mathcal{K}_\ell) \leq O(\sqrt{d \cdot \ell \ln n})$. When these bounds separate, the max eigenvalue distinguishes planted from random.

**Physical interpretation:** $\mathcal{K}_\ell$ is the Hamiltonian of $\ell$ particles with pairwise interactions governed by the original $k$-tensor. As $\ell$ grows, monogamy of entanglement forces the ground state toward a product form $v^{\odot \ell}$, making the spectral relaxation tight — a mean-field theory argument.

## When to reach for it

- Planted inference problems where the signal is hidden in a degree-$k$ polynomial ($k > 2$).
- Certification/refutation problems for random CSPs (e.g., bounding MAX-$k$XOR value).
- Any setting where you need to reduce a tensor optimization problem to a matrix eigenvalue problem.
- As the classical backbone for the [[Guided Sparse Hamiltonian Framework|quantum algorithm]]: the Kikuchi Hamiltonian is the object that gets simulated quantumly.

## Complexity

Classical: computing $\lambda_{\max}(\mathcal{K}_\ell)$ via the Power Method costs $\widetilde{O}(n^\ell)$ time and $\widetilde{O}(n^\ell)$ space. The Kikuchi matrix is never explicitly stored — it's sparse with $O(\ell \log n)$ nonzero entries per row, each computable in $\widetilde{O}(m \ell \log n)$ time.

Quantum: the Kikuchi Hamiltonian can be simulated with $\widetilde{O}(m \ell \log n)$ gates per query, using only $O(\ell \log n)$ qubits.

## Caveat

The spectral gap between planted and random eigenvalues narrows as the constraint density $\Delta$ decreases. Below the computational threshold $\Delta \ll (n/\ell)^{(k-2)/2}$, the method fails regardless of $\ell$. The method also doesn't apply to degree-2 problems (like Planted Clique) — there's nothing to lift.

The mean-field approximation (ground state $\approx$ product state) is not rigorous in general; for $k$XOR, it follows from SoS lower bounds and the specific structure of the Kikuchi graph.

## Related notes
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]]
- [[Input-Derived Guiding State for Planted Problems]]
- [[Guided Sparse Hamiltonian Framework]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
