# Quantum Mean Estimation via Amplitude Estimation

> **Source:** Montanaro, arXiv:1504.06987; builds on Heinrich (2002), Brassard-Høyer-Mosca-Tapp (2002)
> **Tags:** #trick #amplitude-estimation #monte-carlo #mean-estimation

## What it does
Estimates the expected output value of any bounded-variance randomised or quantum algorithm with near-quadratic speedup over classical Monte Carlo sampling.

## The trick

Given algorithm $A$ with $\text{Var}(v(A)) \leq \sigma^2$:

1. **Encode output as amplitude.** For bounded outputs $v(A) \in [0,1]$: append an ancilla, rotate by $\arcsin(\sqrt{\phi(x)})$ conditioned on output $x$. Then $\Pr[\text{ancilla} = |1\rangle] = \mathbb{E}[v(A)]$.

2. **Apply [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]].** With $t$ iterations, estimate the probability to accuracy $O(\sqrt{\mu}/t + 1/t^2)$. Cost: $O(1/\epsilon)$ for additive error $\epsilon$.

3. **Extend to unbounded outputs.** Split the range into dyadic intervals $[2^{\ell-1}, 2^\ell)$. Estimate each interval's contribution separately using the bounded algorithm. Recombine. This handles $\|v(A)\|_2 < \infty$ at cost $\tilde{O}(1/\epsilon)$.

4. **Handle general variance.** Normalise by $\sigma$, take one rough sample for centering, split into positive/negative parts, estimate each, recombine. Total: $\tilde{O}(\sigma/\epsilon)$.

Classical cost: $O(\sigma^2/\epsilon^2)$. Quantum cost: $\tilde{O}(\sigma/\epsilon)$. Optimal by Nayak-Wu.

## When to reach for it
- Estimating expected values of randomised subroutines (Monte Carlo integration)
- Computing partition function ratios via telescoping products
- Any setting where you'd classically take many independent samples and average — the quantum version squares the sample count

## Complexity
$\tilde{O}(\sigma/\epsilon)$ uses of $A$ and $A^{-1}$ for additive error $\epsilon$. For relative error $\epsilon$ with relative variance bound $B$: $\tilde{O}(B/\epsilon)$.

## Caveat
Requires coherent access to $A$ and its inverse $A^{-1}$ (for amplitude estimation). Classical algorithms that use irreversible randomness need to be made reversible first. The quadratic improvement is polynomial, not exponential — the payoff depends on $\sigma/\epsilon$ being large enough to justify quantum hardware overhead.

## Related notes
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Standard Amplitude Amplification]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
