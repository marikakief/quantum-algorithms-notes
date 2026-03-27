
> **Source:** Aaronson, arXiv:quant-ph/0608142 (2006)
> **Tags:** #trick #quantum-learning #PAC-learning #fat-shattering #random-access-codes #sample-complexity

## What it does
Bounds the fat-shattering dimension of the class of quantum states (viewed as real-valued functions of measurements) using quantum random access coding lower bounds, giving a linear-in-$n$ sample complexity for learning quantum states from measurements.

## The trick

**Setup:** Define the p-concept class $\mathcal{C}_n = \{F_\rho : \rho \text{ is an } n\text{-qubit state}\}$ where $F_\rho(E) = \text{Tr}(E\rho)$ for any two-outcome POVM element $E$.

The $\gamma$-fat-shattering dimension $\text{fat}_{\mathcal{C}_n}(\gamma)$ is the largest $k$ such that there exist $k$ measurements $E_1, \ldots, E_k$ that are $\gamma$-fat-shattered: for every $y \in \{0,1\}^k$ there is a state $\rho_y$ with $F_{\rho_y}(E_i) \geq t_i + \gamma$ if $y_i = 1$ and $\leq t_i - \gamma$ if $y_i = 0$, for some thresholds $t_i$.

**Key observation:** A $\gamma$-fat-shattered set of $k$ measurements is exactly a quantum random access code (QRAC): $k$ classical bits encoded into $n$ qubits, with each bit recoverable (from the appropriate measurement) with bias $\gamma$ over $1/2$.

**QRAC lower bound (Ambainis, Nayak, Ta-Shma, Vazirani):** Any QRAC encoding $k$ classical bits into $n$ qubits such that each bit is recoverable with error $\leq \tfrac{1}{2} - \gamma$ requires $n \geq (1 - H(\tfrac{1}{2} - \gamma)) k$, where $H$ is the binary entropy. For small $\gamma$: $n = \Omega(\gamma^2 k)$.

**Conclusion:** $\text{fat}_{\mathcal{C}_n}(\gamma) = O(n/\gamma^2)$.

**Sample complexity:** Classical p-concept learning theory (Anthony & Bartlett; Bartlett & Long) gives:
$$m = O\!\left(\varepsilon^{-1}\!\left(\text{fat}_{\mathcal{C}_n}(\gamma) \cdot \text{polylog} + \log\frac{1}{\delta}\right)\right) = O\!\left(\frac{n}{\varepsilon \gamma^2} \cdot \text{polylog}\right)$$
samples suffice to learn $\rho$ in the sense of predicting $\text{Tr}(E\rho)$ for most $E$ from $D$.

## When to reach for it

- Any time you want a sample complexity bound for a learning problem where the hypothesis class is parameterised by quantum states.
- Proving that some prediction task over quantum states (e.g., estimating acceptance probabilities, predicting channel outputs) is PAC-learnable with polynomially many examples.
- Bounding the expressive power / complexity of quantum models in quantum machine learning.

## Generalisation

The same argument applies to any function class $\{E \mapsto \text{Tr}(A_\rho E)\}$ where $A_\rho$ is an $n$-qubit operator and the QRAC lower bound applies. More broadly: any class of hypotheses that can encode arbitrary bit strings into $n$-dimensional quantum states has fat-shattering dimension $O(n/\gamma^2)$.

## Caveat
- The fat-shattering bound gives a sample complexity upper bound but no algorithm running in time polynomial in $n$ — the consistent hypothesis is found by SDP, which is exponential. The learning guarantee is purely information-theoretic.
- The QRAC lower bound is over independent encoding of each bit; correlated encodings (like quantum error-correcting codes) can do better, but those aren't relevant here because the shattering definition requires one state per labeling.
- Tightness: the lower bound also shows $\text{fat}_{\mathcal{C}_n}(\gamma) = \Omega(n/\gamma^2)$ (Ambainis et al. provide constructions achieving the bound), so the fat-shattering dimension is $\Theta(n/\gamma^2)$.

## Related notes
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — BQP model context; the QRAC lower bound is analogous in spirit to the hybrid argument for query lower bounds
