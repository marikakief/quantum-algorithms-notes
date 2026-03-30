# Quantum Ratio Estimation via Phase Estimation

> **Source:** Wocjan, Chiang, Abeyesinghe, Nagaj, arXiv:0811.0596  
> **Tags:** #trick #phase-estimation #amplitude-estimation #partition-function #quantum-counting

## What it does

Estimates a ratio of partition functions $\alpha_i = Z_{i+1}/Z_i$ to multiplicative accuracy $\varepsilon$ using $O(1/\varepsilon)$ quantum operations instead of $O(1/\varepsilon^2)$ classical samples.

## The trick

Given a [[Coherent Encoding of Classical Distributions for Quantum Speedup|coherent encoding]] $|\pi_i\rangle = \sum_\sigma \sqrt{\pi_i(\sigma)}|\sigma\rangle$ of the Boltzmann distribution at temperature $T_i$, the ratio $\alpha_i = Z_{i+1}/Z_i$ is the expectation

$$\alpha_i = \langle \pi_i | A_i | \pi_i \rangle, \qquad A_i = \sum_\sigma e^{-(\beta_{i+1} - \beta_i)E(\sigma)} |\sigma\rangle\langle\sigma|$$

Encode this into an amplitude: apply a controlled rotation $V_i$ that maps $|0\rangle \to \sqrt{y_i(\sigma)}|1\rangle + \sqrt{1 - y_i(\sigma)}|0\rangle$ on an ancilla, controlled on the system register. Then:

$$\langle\psi_i | (I \otimes |0\rangle\langle 0|) | \psi_i\rangle = \alpha_i$$

Now construct the Grover-like iterate $G = (2|\psi_i\rangle\langle\psi_i| - I)(2P - I)$ where $P = I \otimes |0\rangle\langle 0|$. The eigenvalues of $G$ restricted to the relevant 2D subspace are $e^{\pm i\theta}$ with $\cos\theta = 2\alpha_i - 1$.

Run [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|phase estimation]] on $G$: with $O(1/\varepsilon_{\text{pe}})$ controlled-$G$ applications, extract $\theta$ to precision $\varepsilon_{\text{pe}}$, giving $\alpha_i$ to multiplicative precision $\varepsilon_{\text{pe}}$.

This is a generalisation of [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|quantum counting]]: instead of counting marked items (where $\alpha_i$ is the fraction of marked states), the ratio can be any expectation of a bounded observable encoded as an amplitude.

## When to reach for it

- Estimating partition function ratios in simulated annealing
- Any setting where you need to estimate $\langle \psi | P | \psi \rangle$ for a known projector $P$ and a state $|\psi\rangle$ you can prepare and reflect about
- Quantum speedup of Monte Carlo integration where the integrand is encoded as an amplitude

## Complexity

$O(1/\varepsilon)$ controlled reflections about $|\psi_i\rangle$. The overall cost for a telescoping product of $\ell$ ratios, composed via the powering lemma: $O(\ell^2/\varepsilon \cdot \log \ell)$ reflections.

## Caveat

- Requires the ability to prepare $|\psi_i\rangle$ (and hence $|\pi_i\rangle$) coherently — a mixed-state sample won't suffice, because phase estimation on $G$ needs a pure-state input to produce a meaningful eigenvalue estimate.
- The ancilla rotation $V_i$ must be implementable; this requires that $y_i(\sigma) \in [0, 1]$ for all $\sigma$, which holds when $\alpha_i \leq 1$ (guaranteed by the cooling schedule).
- Success probability per ratio estimate is $7/8$ (before boosting); composition of $\ell$ estimates requires probability boosting via $O(\log \ell)$ repetitions.

## Related notes

- [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Coherent Encoding of Classical Distributions for Quantum Speedup]]
- [[Amplitude Estimation via Phase Estimation on Q]]
