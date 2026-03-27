
> **Source:** Berry, Childs, Cleve, Kothari, Somma, FOCS 2015 (original); applied to chemistry in Babbush et al., arXiv:1506.01029 (2018)
> **Tags:** #trick #LCU #hamiltonian-simulation #taylor-series #precision-scaling #amplitude-amplification

## What it does

Simulates $e^{-iHt}$ by expanding in a truncated Taylor series and implementing the sum as a linear combination of unitaries (LCU). Achieves precision scaling $O(\log 1/\varepsilon)$ — polylogarithmic in $1/\varepsilon$, exponentially better than Trotter's $O(1/\varepsilon^c)$ dependence. This is the main technique underlying the "exponentially more precise" claim of the CI paper.

## The trick

Write the evolution operator as:

$$e^{-iHt} = \sum_{k=0}^{\infty} \frac{(-iHt)^k}{k!}$$

Write $H = \lambda \sum_\ell \alpha_\ell U_\ell$ where $U_\ell$ are unitaries and $\sum_\ell \alpha_\ell = 1$, $\alpha_\ell \geq 0$, and $\lambda = \|H\|_1$ (the 1-norm of the LCU coefficients).

**Step 1 — Truncate:** Approximate with first $K+1$ terms, where $K = O(\log(1/\varepsilon))$ suffices:

$$e^{-iHt} \approx \sum_{k=0}^{K} \frac{(-i\lambda t)^k}{k!} \left(\sum_\ell \alpha_\ell U_\ell\right)^k$$

Expanding $H^k$ gives a sum over $k$-tuples $(\ell_1, \ldots, \ell_k)$ of unitary products.

**Step 2 — Implement as LCU:** Construct two operators on an ancilla space:
- $B$: prepares a superposition over $(k, \ell_1, \ldots, \ell_k)$ weighted by $\sqrt{\frac{(-i\lambda t)^k}{k! \Lambda} \alpha_{\ell_1} \cdots \alpha_{\ell_k}}$ (where $\Lambda = \sum_k (\lambda t)^k/k!$ normalizes)
- $\text{select}(V)$: applies $U_{\ell_1} \cdots U_{\ell_k}$ to the system register conditioned on the ancilla label

The combination gives:

$$(\langle 0 | B^\dagger) \cdot \text{select}(V) \cdot (B|0\rangle) \approx e^{-iHt} / \Lambda$$

**Step 3 — Amplify:** The postselection succeeds with probability $1/\Lambda^2$. Apply oblivious amplitude amplification (repeat $O(\Lambda)$ times) to boost to near-unit success probability.

**Gate cost per time segment $\tau$:**
- $K = O(\log(1/\varepsilon))$ applications of $\text{select}(U_\ell)$
- $r = O(\Lambda t)$ segments needed
- Total: $O(r \cdot K \cdot T_U) = O(\Lambda t \log(1/\varepsilon) \cdot T_U)$ where $T_U$ is the cost of one $\text{select}$ call

**Comparison to Trotter:**

| Method | Precision scaling | Cost (gates) |
|---|---|---|
| Trotter $p$-th order | $O(\varepsilon^{-1/p})$ | $O(\|H\|^{1+1/p} t^{1+1/p} / \varepsilon^{1/p})$ |
| Truncated Taylor series | $O(\log 1/\varepsilon)$ | $O(\Lambda t \log(1/\varepsilon) \cdot T_U)$ |
| QSVT/qubitization | $O(\log 1/\varepsilon)$ | $O(\Lambda t + \log(1/\varepsilon))$ (optimal) |

The Taylor series approach is polynomially better than Trotter in precision and exponentially better in $\varepsilon$-dependence. QSVT later improves on Taylor series by removing the multiplicative $\log(1/\varepsilon)$ factor.

## When to reach for it

- When precision $\varepsilon$ must be very small (many digits) and the polynomial $1/\varepsilon$ cost of Trotter is prohibitive.
- When the Hamiltonian $H$ can be written as a linear combination of efficiently-implementable unitaries (sparse Hamiltonians, chemistry, lattice models).
- As a stepping stone toward qubitization/QSVT: Taylor series is simpler to implement than QSVT and achieves the important $\log(1/\varepsilon)$ precision scaling.
- For fault-tolerant algorithms where circuit depth (not qubit count) is the bottleneck.

## Complexity

- **Ancilla qubits:** $O(\log L + \log K)$ where $L$ is the number of LCU terms and $K$ is the truncation order
- **Truncation order:** $K = O\!\left(\log\frac{1}{\varepsilon} / \log\log\frac{1}{\varepsilon}\right)$ — essentially $O(\log 1/\varepsilon)$
- **Total gate count:** $O(\Lambda t \log(1/\varepsilon) \cdot T_{\text{select}})$, where $\Lambda = \|H\|_1$ is the LCU 1-norm and $T_{\text{select}}$ is the cost of one select call
- **For CI chemistry (Babbush et al.):** $\Lambda = \tilde{O}(\eta^2 N^2)$, $T_{\text{select}} = \tilde{O}(N)$, giving $\tilde{O}(\eta^2 N^3 t)$

## Caveat

- The 1-norm $\Lambda$ can be large even when $\|H\|$ (spectral norm) is small. For chemistry, $\Lambda$ scales as $O(N^4)$ in second-quantized and $O(\eta^2 N^2)$ in first-quantized, so there is still polynomial overhead.
- Oblivious amplitude amplification requires the state preparation oracle $B$ to be efficient and the success probability $1/\Lambda$ not exponentially small. If $\Lambda$ is exponentially large, the method fails.
- For real chemistry simulations, the constants hidden in $\tilde{O}$ matter. Practical estimates suggest fault-tolerant resource requirements are still very large.
- Superseded for [[Hamiltonian simulation]] by qubitization/QSVT in terms of asymptotic gate complexity (by a $\log(1/\varepsilon)$ factor). However, QSVT is conceptually and technically harder to implement.

## Related notes
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — applies this technique to real-space position-grid simulation; $\tilde{O}(\eta^2)$ pairwise queries for fixed $h$
- [[CI Matrix Simulation (First-Quantized Encoding)]]
- [[On-the-fly Molecular Integral Evaluation]]
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]]
- [[Real-Space Grid Encoding for Many-Body Simulation]]
- [[High-Order Finite Difference Kinetic Energy]]
- [[Trotterized Time Evolution for Chemistry]] — what this supersedes in precision scaling
- [[Iterative Phase Estimation (Kitaev)]] — outer wrapper for eigenvalue estimation
