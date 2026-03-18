# Payoff Encoding via Ancilla Rotation

> **Source:** Rebentrost-Gupt-Bromley, arXiv:1805.00109; technique appears broadly in quantum Monte Carlo literature (Brassard-Høyer-Mosca-Tapp 2002, Montanaro 2015)
> **Tags:** #trick #amplitude-estimation #monte-carlo #function-encoding

## What it does
Encodes a real-valued function $v(x) \in [0,1]$ into the amplitude of an ancilla qubit, so that the probability of measuring $|1\rangle$ equals the expected value $\mathbb{E}[v]$ under the input distribution.

## The trick

Given a quantum state $|\psi\rangle = \sum_x a_x |x\rangle$ encoding a probability distribution ($|a_x|^2 = p(x)$), apply a controlled rotation on an ancilla:

$$R|x\rangle|0\rangle = |x\rangle\left(\sqrt{1 - v(x)}|0\rangle + \sqrt{v(x)}|1\rangle\right)$$

The combined state is:

$$|\chi\rangle = \sum_x a_x |x\rangle\left(\sqrt{1-v(x)}|0\rangle + \sqrt{v(x)}|1\rangle\right)$$

Then:

$$\Pr[\text{ancilla} = |1\rangle] = \sum_x |a_x|^2 v(x) = \mathbb{E}[v(A)]$$

This probability is exactly the quantity we want. Apply [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] to extract it with $O(1/\epsilon)$ precision — quadratic improvement over repeated measurement.

**Implementation:** The rotation angle $\theta_x = \arcsin(\sqrt{v(x)})$ must be computed coherently. For simple functions (e.g., linear payoffs, indicator functions), this requires quantum arithmetic (comparators, adders, controlled rotations). For complex functions, approximate with piecewise-linear or polynomial fits.

## When to reach for it
- Any quantum Monte Carlo problem where you want $\mathbb{E}[f(X)]$ for a bounded function $f$
- Financial derivative pricing (payoff functions)
- Estimating observables of quantum or classical distributions
- Any setting where amplitude estimation replaces classical sampling

## Complexity
The rotation $R$ itself adds $O(\text{poly}(n))$ gates for quantum arithmetic on $n$-qubit registers. The total cost is dominated by [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]: $O(1/\epsilon)$ applications of the prepare-and-rotate circuit.

## Caveat
The function $v(x)$ must be rescaled to $[0,1]$. If the original function has large range (e.g., a call option payoff that can be arbitrarily large), you need to normalise by a known upper bound, which enters the final error. If $v(x) \notin [0,1]$, use the dyadic decomposition from [[Quantum Mean Estimation via Amplitude Estimation|quantum mean estimation]] to handle unbounded outputs. Also, computing $v(x)$ coherently in quantum arithmetic can be expensive — the "simple rotation" hides real circuit depth.

## Related notes
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
