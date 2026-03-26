# Carleman Linearisation for Quantum Nonlinear ODE Solvers

> **Source:** Carleman (1932); quantum application by Liu-Kolden-Krovi-Loureiro-Trivisa-Childs (2021); generalised to arbitrary $M$ in Costa-Schleich-Morales-Berry, arXiv:2312.09518
> **Tags:** #trick #nonlinear-ODE #Carleman-linearisation #linearisation #Kronecker

## What it does
Converts a polynomial nonlinear ODE into an infinite-dimensional linear ODE, which can be truncated and solved with quantum linear system methods.

## The trick

Given $\dot{\mathbf{u}} = F_1\mathbf{u} + F_M\mathbf{u}^{\otimes M}$ with $\mathbf{u} \in \mathbb{R}^n$, define $y_j = \mathbf{u}^{\otimes j}$ for $j = 1, 2, \ldots$ and collect them into a vector $\mathbf{y} = [y_1, y_2, \ldots]^T$. The time derivative of each $y_j$ involves only $y_j$ (from the linear term) and $y_{j+M-1}$ (from the nonlinear term):

$$\frac{\mathrm{d}y_j}{\mathrm{d}t} = A_j^{(1)} y_j + A_{j+M-1}^{(M)} y_{j+M-1},$$

where $A_j^{(1)} = \sum_{\ell=1}^{j} I^{\otimes(\ell-1)} \otimes F_1 \otimes I^{\otimes(j-\ell)}$ and similarly for $A_{j+M-1}^{(M)}$ with $F_M$.

Truncating at order $N$ gives a finite linear system $\dot{\mathbf{y}} = A_N \mathbf{y}$ with the block-banded Carleman matrix $A_N$. The dimension is $N_{\mathrm{tot}} = \sum_{j=1}^{N} n^j$, exponential in $N$ — which is fine for a quantum computer (it's $O(N\log n)$ qubits) but prohibitive classically.

The solution $\mathbf{u}(T)$ is the first component $y_1(T)$ of the Carleman vector.

## When to reach for it

- Solving nonlinear ODEs with polynomial nonlinearities on a quantum computer
- PDEs after spatial discretisation (reaction-diffusion, Burgers, etc.) that produce polynomial nonlinear ODE systems
- Any setting where $R = \|F_M\|\|\mathbf{u}_{\mathrm{in}}\|^{M-1}/|\lambda_0| < 1$ — i.e., the dissipative term dominates the nonlinearity

## Complexity

The Carleman order needed for error $\varepsilon$ is $N = O((M-1)\log(1/\varepsilon)/\log(1/R))$, so logarithmic in precision. The total Hilbert space dimension is $n^N$, encoded in $N\log n$ qubits. The [[Block-Encoding Composition Algebra|block-encoding]] normalisation is $\lambda_{A_N} = O(N\|F_1\|)$.

## Caveat

- Requires $R < 1$ (weak nonlinearity, strong dissipation). For $R \geq \sqrt{2}$, the nonlinear ODE problem is BQP-hard.
- Without [[Carleman Vector Rescaling for Measurement Probability Boost|rescaling]], the probability of measuring the solution component $y_1$ is $O(\|\mathbf{u}_{\mathrm{in}}\|^{-2N})$ — exponentially small in $N$. Rescaling is not optional.
- Only handles polynomial nonlinearities. Non-polynomial $f(\mathbf{u})$ must be approximated by Taylor expansion first.
- The truncated linear system only approximates the true nonlinear dynamics. Qualitative nonlinear phenomena (bifurcations, chaos, blowup) are not captured.

## Related notes
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[Carleman Vector Rescaling for Measurement Probability Boost]]
- [[Norm Tension Between PDE Stability and Quantum Rescaling]]
- [[History-State Linear System Encoding for ODE Trajectories]] — used to solve the resulting linear ODE
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the original quantum linear system solver
