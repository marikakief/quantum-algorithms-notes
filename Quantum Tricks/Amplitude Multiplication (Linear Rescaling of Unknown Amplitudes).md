# Amplitude Multiplication (Linear Rescaling of Unknown Amplitudes)

> **Source:** Low & Chuang, arXiv:1707.05391  
> **Tags:** #trick #amplitude-amplification #QSP #state-preparation

## What it does

Rescales an unknown state amplitude $\lambda$ to $\lambda/(2\Gamma)$ with only multiplicative error $\epsilon$, independent of the value of $\lambda$. Contrast with standard [[Amplitude Amplification and Estimation|amplitude amplification]], which applies the nonlinear map $\lambda \mapsto \sin((2N+1)\sin^{-1}(\lambda))$.

## The trick

Start with a state preparation unitary $\hat{G}|0\rangle = \lambda|t\rangle|0\rangle + \sqrt{1-\lambda^2}|t^\perp\rangle$ and an upper bound $\Gamma \geq |\lambda|$.

1. Use [[Flexible QSP via A-Component Cancellation|flexible amplitude amplification]] (Theorem 7 of the paper): the same controlled-superposition trick as flexible QSP, but for state preparation. Control the iterate $\hat{V}_{\vec{\phi}}$ and $\hat{V}_{\pi-\vec{\phi}}$ on an ancilla to cancel the $C$ component, leaving only $D(\lambda)$ as the target-state amplitude.

2. Choose $D$ to approximate the truncated linear function:
$$f_{\text{lin},\Gamma}(x) = \begin{cases} x/(2\Gamma), & |x| \leq \Gamma \\ \in [-1,1], & |x| > \Gamma \end{cases}$$

3. The polynomial approximation (Appendix A of the paper) constructs $p_{\text{lin},\Gamma,n}$ of degree $n = O(\Gamma^{-1}\log(1/\epsilon))$ using error functions and Chebyshev truncation.

The output state has amplitude $\lambda/(2\Gamma)$ on the target, with multiplicative error $\epsilon|\lambda|/(2\Gamma)$.

The linearity is everything. Standard amplitude amplification maps $\lambda \mapsto \sin((2N+1)\sin^{-1}(\lambda))$, which introduces state-dependent errors that compound over long simulation times (this is why the Berry-Childs 2012 approach pays $O(t^{3/2}/\sqrt{\epsilon})$). Amplitude multiplication avoids this entirely.

## When to reach for it

- Building [[Standard-Form Encoding (Prepare + Signal Oracle)|standard-form encodings]] where the state preparation has a "slowdown factor" — the amplitude in the good subspace is $\sqrt{\lambda_\beta} \ll 1$ — and you need to boost it uniformly across all indices.
- Any setting where the nonlinearity of standard [[Amplitude Amplification and Estimation|amplitude amplification]] creates errors that compound (particularly inside [[Hamiltonian simulation]] algorithms).
- As a subroutine for [[Uniform Spectral Amplification of Low-Energy Subspaces|uniform spectral amplification]] of state-overlap Hamiltonians.

## Complexity

$O(\Gamma^{-1}\log(1/\epsilon))$ queries to $\hat{G}$ and $\hat{G}^\dagger$, $O(\Gamma^{-1}\log(1/\epsilon)\log d)$ primitive gates, 1 extra ancilla qubit.

## Caveat

Only works for $|\lambda| \leq 1/2$ and $\Gamma \leq 1/2$ (the polynomial must stay bounded on $[-1,1]$, and the truncated linear function $x/(2\Gamma)$ touches $\pm 1$ at $x = \pm \Gamma$). For $\Gamma > 1/2$, multiplication by a constant less than 1 is trivial — just prepare a weighted ancilla.

Also requires the state preparation oracle $\hat{G}$ to mark the target with a flag qubit $|0\rangle_b$ (for the controlled reflections in the iterate), which is standard but worth noting.

## Related notes
- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]]
- [[Flexible QSP via A-Component Cancellation]]
- [[Uniform Spectral Amplification of Low-Energy Subspaces]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Amplitude Amplification and Estimation]]
- [[Standard Amplitude Amplification]]
- [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]]
