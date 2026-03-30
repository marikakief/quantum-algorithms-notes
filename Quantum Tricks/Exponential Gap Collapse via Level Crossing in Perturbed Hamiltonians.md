# Exponential Gap Collapse via Level Crossing in Perturbed Hamiltonians

> **Source:** van Dam, Mosca, Vazirani, arXiv:quant-ph/0206003
> **Tags:** #trick #adiabatic #spectral-gap #lower-bound #perturbation-theory

## What it does
Proves that adding a perturbation to an adiabatic Hamiltonian — one that moves the global minimum to a subspace with exponentially small overlap with the unperturbed ground state — collapses the spectral gap to exponentially small, forcing exponential runtime.

## The trick

Start with a "nice" Hamiltonian $H_w(t)$ whose gap is $\Omega(1)$ and whose ground state $|v_0(s)^{\otimes n}\rangle$ has exponentially small overlap with some target state $|1^n\rangle$:

$$|\langle 1^n | v_0(s)^{\otimes n}\rangle| \leq \frac{1}{\sqrt{2^n}} \quad \text{for all } s$$

Now perturb: $H_f(t) = H_w(t) - s(n+1)|1^n\rangle\langle 1^n|$, making $|1^n\rangle$ the global minimum of $H_f(T)$ with eigenvalue $-1$.

**The argument:**
1. Transform to the eigenbasis of $H_w$: $A = V^\dagger H_f V$
2. Decouple the ground-state direction by zeroing the off-diagonal couplings to $|v_0^{\otimes n}\rangle$, obtaining matrix $B$
3. $B$ has a guaranteed zero gap at some critical $s_c$ (where the unperturbed ground energy crosses the perturbed energy of $|1^n\rangle$)
4. $\|A - B\|_2 \leq s(n+1)/\sqrt{2^{n-1}}$ — exponentially small, because the perturbation couples to the ground state only through the tiny overlap $\langle 1^n | v_0^{\otimes n}\rangle$
5. By the Bhatia matching distance bound: $g_{\min}(A) \leq g_{\min}(B) + 2\|A-B\|_2 = O(n/\sqrt{2^n})$

Therefore $T = \Omega(g_{\min}^{-2}) = \Omega(2^n/n^2)$.

## When to reach for it

- Proving exponential lower bounds for specific adiabatic algorithms
- Demonstrating that local-minimum traps (broad basin at wrong answer, narrow basin at correct answer) cause exponential slowdown in AQC
- Constructing hard instances for quantum annealing benchmarks
- Understanding when adiabatic paths fail due to avoided crossings with exponentially small coupling

## Complexity

The gap is $O(\text{poly}(n)/\sqrt{2^n})$, giving runtime $\Omega(2^n/\text{poly}(n))$.

## Caveat

This is a worst-case construction — the perturbation is adversarially designed to exploit the exponentially small overlap. It doesn't say AQC always fails; it says the linear interpolation path can be trapped. Alternative paths, auxiliary Hamiltonians, or non-stoquastic drivers might avoid the trap.

The construction requires the ground state to have exponentially small support on the subspace where the minimum moves to. Product-state ground states (like the Hamming weight case) have this property, but more complex ground states might not.

## Related notes
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Local Adiabatic Schedule for Gap Navigation]]
