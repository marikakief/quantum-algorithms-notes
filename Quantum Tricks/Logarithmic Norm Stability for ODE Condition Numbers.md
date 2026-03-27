
> **Source:** Berry and Costa, arXiv:2212.03544; see also Krovi, arXiv:2202.01054
> **Tags:** #trick #stability #condition-number #ODE #linear-systems

## What it does
Bounds the condition number of the block-bidiagonal linear system arising from a quantum ODE solver using the *logarithmic norm* of $A(t)$, rather than requiring $A(t)$ to be diagonalisable or its eigenvalues to have non-positive real part.

## The trick

The key identity: for the time-ordered exponential $W(t, t_0) = \mathcal{T}\exp\!\left(\int_{t_0}^t A(s)\,ds\right)$,

$$\|W(t, t_0)\| \le \exp\!\left(\int_{t_0}^t \mu(A(s))\,ds\right)$$

where $\mu(A) = \lambda_{\max}\!\left(\frac{A + A^\dagger}{2}\right)$ is the logarithmic norm (also called the matrix measure).

If $\mu(A(t)) \le 0$ for all $t$, then $\|W(t,t_0)\| \le 1$. This directly implies:
- $\|V_m\| \le 1 + \varepsilon$ (approximate propagator bounded)
- $\|\mathcal{A}\| = O(1)$ (block matrix norm bounded)
- $\|\mathcal{A}^{-1}\| = O(R)$ (inverse norm bounded by number of time steps)
- $\kappa_{\mathcal{A}} = O(R)$ (condition number linear in the number of steps)

The logarithmic norm condition is a different criterion from requiring all eigenvalues of $A(t)$ to have non-positive real part:
- For normal $A$, the two conditions coincide: $\mu(A) = \max_i \text{Re}(\lambda_i)$
- For non-normal $A$, $\mu(A)$ can exceed $\max_i \text{Re}(\lambda_i)$ — so the eigenvalue condition is *weaker* (easier to satisfy) than the logarithmic norm condition
- However, $\mu(A) \le 0$ directly implies $\|e^{At}\| \le 1$ without needing to diagonalise $A$. The eigenvalue condition alone doesn't guarantee bounded $\|e^{At}\|$ for non-normal matrices (transient growth can be arbitrarily large)

The real advantage: no diagonalisation needed. [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] used an explicit diagonalisation $A = P^{-1}DP$, introducing a factor of $\kappa(P)$ (the condition number of the eigenvector matrix). The logarithmic norm approach eliminates this entirely.

## When to reach for it

- Any quantum algorithm for dissipative linear dynamics where you need to bound the condition number of a history-state linear system
- Proving complexity bounds for non-normal coefficient matrices
- When the coefficient matrix $A(t)$ is far from normal and $\kappa(P)$ would be large

## Complexity

No direct complexity cost — this is an analysis tool. It gives $\kappa_{\mathcal{A}} = O(\lambda_A T)$ under $\mu(A(t)) \le 0$, which translates to $O(\lambda_A T \log(1/\varepsilon))$ QLSA calls.

## Caveat

The logarithmic norm condition is sufficient but not necessary. There exist stable ODEs where $\mu(A(t)) > 0$ at some times but the overall dynamics is still bounded. The paper notes that "other conditions bounding the norm of the time-ordered exponential can be used" — this is the general case where you might bound $\|W(t,t_0)\|$ directly.

For time-independent $A$, Krovi (2023) discusses this condition in detail. For time-dependent $A$, the logarithmic norm bound via the integral is a natural extension.

## Related notes
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[History-State Linear System Encoding for ODE Trajectories]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
