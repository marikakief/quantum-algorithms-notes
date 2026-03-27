
> **Source:** Aharonov, van Dam, Kempe, Landau, Lloyd, Regev, arXiv:quant-ph/0405098, Lemma 3.10
> **Tags:** #trick #history-state #amplification

## What it does

Boosts the probability of obtaining the final circuit output from a [[History-State Encoding with Unary Clock|history state]], without repeating the computation.

## The problem

The history state $|\eta\rangle = \frac{1}{\sqrt{L+1}} \sum_\ell |\alpha(\ell)\rangle \otimes |1^\ell 0^{L-\ell}\rangle$ gives the final state $|\alpha(L)\rangle$ with probability $1/(L+1)$ when the clock is measured. That's wasteful.

## The trick

Append $(2/\epsilon - 1)L$ identity gates to the end of the circuit. The new circuit has $L' = 2L/\epsilon$ gates, but the last $(L' - L)$ steps all produce the same state $|\alpha(L)\rangle$.

Now measuring the clock gives a time step $\ell \geq L$ with probability $(L' - L + 1)/(L' + 1) \geq 1 - \epsilon$. In all those cases, the computation register holds $|\alpha(L)\rangle$.

```
 Original circuit          Padded identity gates
 ├─── L gates ───┤├────── (2/ε - 1)L identity gates ──────┤

 |α(0)⟩ → ··· → |α(L)⟩ → |α(L)⟩ → |α(L)⟩ → ··· → |α(L)⟩

                           ↑ Most of the history state
                             amplitude is here
```

## Quantitative (Lemma 3.10)

If an adiabatic algorithm can produce a state $\epsilon$-close to the [[History-State Encoding with Unary Clock|history state]] of any $L$-gate circuit with running time $f(L, \epsilon)$, then it can produce a state $\epsilon$-close to the final circuit output (in trace distance) with running time $f(2L/\epsilon, \epsilon/2)$.

## When to use it

- Any [[History-State Encoding with Unary Clock|history-state encoding]] where you want the final-time output with high probability
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Quantum ODE/PDE algorithms]] that encode solution trajectories in history states
- Whenever the "measure the clock and hope" approach has too low a success probability

## Cost

Increases the total number of time steps by a factor of $O(1/\epsilon)$. Since adiabatic running time scales as a polynomial in $L'$, this introduces polynomial overhead in $1/\epsilon$.

## Connection to ODE algorithms

This is the ancestor of the [[History-State Padding for Final-Time Readout]] trick used in [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|quantum linear ODE solvers]]. The idea is the same: pad the trajectory so most of the history-state amplitude sits at the time you care about.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[History-State Encoding with Unary Clock]]
- [[History-State Padding for Final-Time Readout]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
