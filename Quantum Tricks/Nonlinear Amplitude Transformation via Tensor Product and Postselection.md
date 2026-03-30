# Nonlinear Amplitude Transformation via Tensor Product and Postselection

> **Source:** Leyton, Osborne, arXiv:0812.4423 (2008)
> **Tags:** #trick #nonlinear #amplitude-transformation #postselection #tensor-product

## What it does
Implements a polynomial map $z \mapsto f(z)$ on the probability amplitudes of a quantum state, despite quantum mechanics being linear. The output state has amplitudes that are polynomial functions of the input state's amplitudes.

## The trick

Quantum mechanics forbids nonlinear evolution, but tensor products generate monomials. Given $|\phi\rangle = \sum_j z_j |j\rangle$, the state $|\phi\rangle^{\otimes k}$ has amplitudes $z_{j_1} z_{j_2} \cdots z_{j_k}$ — every monomial of degree $\leq k$ in the $z_j$ is present.

For a quadratic map $f_\alpha(z) = \sum_{k,l} a_{kl}^{(\alpha)} z_k z_l$:

1. Take two copies: $|\phi\rangle|\phi\rangle = \sum_{j,k} z_j z_k |j\rangle|k\rangle$
2. Apply the linear operator $A = \sum_{\alpha,k,l} a_{kl}^{(\alpha)} |\alpha 0\rangle\langle kl|$ to extract the coefficients
3. Since $A|\phi\rangle|\phi\rangle \propto \sum_\alpha f_\alpha(z)|\alpha\rangle|0\rangle = |\phi'\rangle|0\rangle$, the desired nonlinear transformation is achieved

The operator $A$ is not unitary, so implementation requires embedding in a Hamiltonian $H = -iA \otimes |1\rangle\langle 0| + iA^\dagger \otimes |0\rangle\langle 1|$, evolving for a short time $\varepsilon$, and postselecting on the pointer qubit being $|1\rangle$. Success probability is $O(\varepsilon^2)$.

For degree-$d$ polynomial maps, consume $d$ copies per step instead of 2.

## When to reach for it

- Implementing nonlinear dynamics on quantum state amplitudes (ODE integration, dynamical systems)
- Any time you need to compute a polynomial function of unknown quantum amplitudes
- As a building block for quantum algorithms that require going beyond linear quantum mechanics

## Complexity

Each application succeeds with probability $O(\varepsilon^2)$, where $\varepsilon \leq 1/\|H\|$ is the evolution time. For $m$ sequential applications (e.g., $m$ Euler steps), the total number of initial copies needed is $(C/\varepsilon^2)^m$ — **exponential in $m$**.

The Hamiltonian simulation cost per step is $\text{poly}(\log n)$ when $A$ is $O(1)$-sparse.

## Caveat

- The exponential copy count is the fundamental bottleneck. This was the core limitation of [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes|Leyton-Osborne (2008)]] and the reason [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] was adopted instead: it avoids repeated nonlinear amplitude transformations entirely by linearising the problem upfront.
- "Poisoned" states from failed postselection attempts are discarded. Recovery schemes could potentially reduce waste, but none are known.
- The measure-preservation assumption ($\sum_j |z_j|^2 = 1$ throughout) is needed for the success probability analysis. Without it, success probability can degrade further at each step.
- No-cloning prevents reusing failed states, and the copies of $|\phi(t)\rangle$ at intermediate times must be freshly produced from the previous round's successes.

## Related notes
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]]
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the approach that replaced this technique for practical nonlinear ODE quantum algorithms
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
