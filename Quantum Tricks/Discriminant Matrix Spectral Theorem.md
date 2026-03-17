
> **Source:** Szegedy, FOCS 2004, Theorem 1
> **Tags:** #trick #quantum-walk #spectral #fundamental

## What it does

Gives the complete spectral decomposition of a product of two reflections $\mu = \mathrm{ref}_B \cdot \mathrm{ref}_A$ in terms of the **discriminant matrix** — a Gram-matrix-like object that encodes the overlap between the two subspaces.

## The theorem

Let $A = \langle v_1, \ldots, v_n \rangle$ and $B = \langle w_1, \ldots, w_m \rangle$ be subspaces with orthonormal bases. Define the discriminant matrix:

$$
M = \mathrm{Gram}(v_1, \ldots, v_n, w_1, \ldots, w_m) - I_{n+m}
$$

(so $M[v_i, w_j] = \langle v_i, w_j \rangle$ and diagonal blocks are zero).

If $\lambda$ is an eigenvalue of $M$ with $0 < \lambda < 1$ and eigenvector $(a, b)$, then $\mu$ has eigenvalues:

$$
e^{\pm i\theta} = 2\lambda^2 - 1 \pm 2i\lambda\sqrt{1-\lambda^2}, \qquad \theta = \arccos(2\lambda^2 - 1)
$$

with eigenvectors $\tilde{a} - \lambda\tilde{b} \pm i\sqrt{1-\lambda^2}\,\tilde{b}$.

**Special cases:**
- $\lambda = 1$: $\mu$ eigenvalue 1 (vectors in $A \cap B$)
- $\lambda = 0$: $\mu$ eigenvalue $-1$ (vectors in $A$ or $B$ that are orthogonal to the other)

The map $\lambda \mapsto e^{\pm i\arccos(2\lambda^2-1)}$ folds $[-1,1]$ onto the unit circle.

## Why it matters

This is the spectral relationship that makes quantum walks work. It maps classical Markov chain eigenvalues (the $\lambda$'s in the discriminant) to quantum walk eigenphases. The quadratic relationship $\theta \approx \sqrt{2(1-\lambda)}$ for $\lambda$ near 1 is what produces the quadratic speedup in the $\sqrt{\delta\epsilon}$ rule.

This same spectral relationship reappears in [[Qubitization Iterate|qubitization]], where the discriminant matrix eigenvalues are the eigenvalues of the [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoded]] Hamiltonian, and [[QSVT Meta-Template|QSVT]] applies polynomial transforms to $\theta$.

## When to reach for it

- Analyzing any [[Quantized Bipartite Walk Construction|quantized walk]] operator
- Understanding the eigenvalue structure of [[Qubitization Iterate|qubitization]] iterates
- Computing the phase gap for quantum walk search algorithms

## Caveat

The eigenvectors of $\mu$ are not orthonormal unless properly scaled — each pair has norm $\sqrt{1-\lambda^2}$ (Theorem 5 in the paper). This matters when decomposing states in the eigenbasis.

## Related notes

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Quantized Bipartite Walk Construction]]
- [[Qubitization Iterate]]
- [[QSVT Meta-Template]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
