
> **Tags:** #trick #state-prep #linear-odes #history-state
> **Source:** Aharonov et al. arXiv:quant-ph/0405098 (Lemma 3.10); also arXiv:1701.03684

## What it does

Boosts the probability of reading out the final-time state from a [[History-State Encoding with Unary Clock|history state]] encoding, without classical post-processing or repetition.

## The trick

A [[History-State Encoding with Unary Clock|history state]] $|\eta\rangle = \frac{1}{\sqrt{L+1}} \sum_{\ell=0}^{L} |\alpha(\ell)\rangle \otimes |\mathrm{clock}=\ell\rangle$ gives the final state $|\alpha(L)\rangle$ with probability $1/(L+1)$ when the clock is measured.

**Fix:** Append $(2/\epsilon - 1)L$ identity gates to the circuit. The modified [[History-State Encoding with Unary Clock|history state]] has $L' = 2L/\epsilon$ steps, of which $(2/\epsilon - 1)L$ carry the same final state $|\alpha(L)\rangle$. Measuring the clock now gives $\ell \geq L$ (and thus $|\alpha(L)\rangle$) with probability $\geq 1 - \epsilon$.

The adiabatic evolution time increases polynomially ($L \to L' = 2L/\epsilon$) but the readout goes from $1/(L+1)$ to $1 - \epsilon$.

## When to reach for it

- Any [[History-State Encoding with Unary Clock|history state]] encoding where direct readout probability is $1/(L+1)$
- Quantum ODE/PDE algorithms (Berry-Childs-Ostrander-Wang 2017)
- Adiabatic simulations of circuits (Aharonov et al. 2004)

## Complexity

Increases system size from $L$ to $O(L/\epsilon)$. All polynomial costs that depend on $L$ pick up a factor of $1/\epsilon$.

## Caveat

Increases system size modestly; must rebalance condition number impact in ODE/linear-system settings. The identity padding is "wasted computation" — you're doing nothing for most of the circuit just to concentrate amplitude.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Identity-Gate Padding for Output Amplification]]
- [[History-State Encoding with Unary Clock]]
