# Grassmann Integral Framework for Free-Fermion Traces

> **Source:** Wan, Huggins, Lee, Babbush, arXiv:2207.13723
> **Tags:** #trick #Grassmann-integral #free-fermion #Pfaffian #classical-simulation #matchgate #Clifford-algebra

## What it does

Evaluates $\operatorname{tr}(A^{(1)} \cdots A^{(m)})$ in polynomial time whenever each $A^{(i)}$ is a Majorana product, a fermionic Gaussian unitary, or a fermionic Gaussian density matrix — by reducing the trace to a Grassmann integral of a specific form that can be computed via Pfaffians.

## The trick

### Step 1: Trace → Grassmann integral (Theorem 4)

For even $m$ and arbitrary $A^{(1)}, \ldots, A^{(m)} \in L(\mathcal{H}_n)$, introduce $m$ independent sets of $2n$ Grassmann variables $\theta^{(1)}, \ldots, \theta^{(m)}$:

$$\operatorname{tr}(A^{(1)} \cdots A^{(m)}) = 2^n (-1)^{nm(m-1)/2} \int D\theta^{(m)} \cdots D\theta^{(1)} \prod_{i=1}^{m} \omega(A^{(i)}; \theta^{(i)}) \exp\!\left(\sum_{i < j} s_{ij} \theta^{(i)T} \theta^{(j)}\right)$$

where $\omega(A; \theta)$ is the Grassmann representation of $A$ (replace each Majorana $\gamma_\mu$ with Grassmann variable $\theta_\mu$) and $s_{ij} = (-1)^{i+j+1}$.

The idea: operator multiplication in the Clifford algebra involves both the wedge product and contractions (from $\gamma_\mu^2 = I$). In the Grassmann algebra, $\theta_\mu^2 = 0$, so using independent generators per operator preserves terms that would vanish from squaring. The exponential coupling term compensates by correctly weighting the cross-terms.

### Step 2: Assemble into standard form

For the three operator types:

- **Majorana products** $\tilde{\gamma}_S = \tilde{\gamma}_{\mu_1} \cdots \tilde{\gamma}_{\mu_k}$: their Grassmann representations are products of linear forms $(\tilde{Q}\theta^{(i)})_{\mu_1} \cdots (\tilde{Q}\theta^{(i)})_{\mu_k}$, encoded as rows of the matrix $B$.
- **Gaussian density matrices** $\varrho$: contribute $\exp(-\frac{i}{2} \theta^{(i)T} C_\varrho \, \theta^{(i)})$, folded into the quadratic form matrix $M$.
- **Gaussian unitaries** $U_R$ (with $R \in SO(2n)$): $\omega(U_R; \theta) = c \exp(\frac{1}{2} \theta^T T_R \theta)$ where $T_R = Q'^T \operatorname{diag}(\tan \sigma_j) Q'$ from the Hamiltonian $H = \frac{i}{2} \sum h_{\mu\nu} \gamma_\mu \gamma_\nu$ with eigenvalues $\sigma_j$. Also folds into $M$. (When $\cos \sigma_j = 0$ for some $j$, those bivectors go into $B$ instead.)

Combine all exponentials and the coupling term into a single $2mn \times 2mn$ antisymmetric matrix $M$ (block-structured). Collect all linear forms into a $K \times 2mn$ matrix $B$. The result:

$$\operatorname{tr}(\cdots) \propto g(B, M) := \int D\chi \, (B\chi)_1 \cdots (B\chi)_K \exp\!\left(\frac{1}{2} \chi^T M \chi\right)$$

### Step 3: Evaluate $g(B, M)$ via Algorithm 2

- If $K$ odd or $K > 2N$: return 0.
- If $K = 2N$: return $\det(B)$.
- If $M$ invertible: $g(B, M) = \operatorname{pf}(M) \, \operatorname{pf}(-B M^{-1} B^T)$.
- If $M$ singular: reduce recursively. Factor $M = R^T \begin{pmatrix} M' & 0 \\ 0 & 0 \end{pmatrix} R$ where $M'$ is the invertible part. Split $BR = (B' \mid B'')$. Then $g(B, M) = (-1)^{N+K/2} \operatorname{pf}(-M') \cdot g(B''^T, B' M'^{-1} B'^T)$.

Each recursion step reduces the "outer" dimension by at least 2, so at most $N$ steps. Total cost: $O(N^4)$ for general $M$, $O(N^3)$ when $M$ is invertible.

## When to reach for it

- Computing matchgate shadow estimates for observables beyond local Majorana products and Gaussian fidelities — e.g., overlaps $\langle \psi | \varphi \rangle$ where $|\varphi\rangle$ is an arbitrary pure Gaussian state (not a Slater determinant).
- Any classical simulation task involving traces of products of free-fermion operators. The framework is a general-purpose tool for evaluating free-fermion quantities.
- Deriving new minor summation formulas: expanding the trace via Wick's theorem and equating with the Pfaffian output yields combinatorial identities for free.

## Complexity

- **Assembling $B$ and $M$:** $O(mn^2)$ per operator (Gaussian unitaries require an eigendecomposition of the Hamiltonian coefficient matrix: $O(n^3)$).
- **Evaluating $g(B, M)$:** $O((mn)^3)$ when $M$ is invertible, $O((mn)^4)$ in general.
- **For matchgate shadow post-processing ($m = 4$):** $O(n^4)$ per shadow sample (the matrix is $8n \times 8n$).

## Caveat

The polynomial interpolation step (evaluating $g(B, M(z))$ at $O(n)$ values of $z$ to extract grade-$\ell$ coefficients) multiplies the cost by $n$. So the total per-sample cost for overlap estimation via this route is $O(n^5)$ in the worst case — worse than the $O(n^4)$ from Theorem 3's direct Pfaffian polynomial. The general framework is most useful for observables where no specialised formula exists.

Variance bounds for the general framework are not provided. The paper bounds variance only for local operators, Gaussian fidelities, and Slater determinant overlaps.

## Related notes

- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]]
- [[Generating-Function Pfaffian Trick for Gaussian Overlaps]] — the specialised (faster) version for Gaussian-Gaussian traces
- [[Pfaffian Shadow Estimation for Fermionic Observables]] — the specialised version for local Majorana products
- [[Matchgate 3-Design for Classical Shadows]] — ensures variance expressions hold for both discrete and continuous ensembles
- [[Virtual Correlation via Matchgate Contraction]] — another exploitation of matchgate/free-fermion structure for efficient classical computation
