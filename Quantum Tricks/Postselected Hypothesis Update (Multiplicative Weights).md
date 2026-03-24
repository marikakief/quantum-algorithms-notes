# Postselected Hypothesis Update (Multiplicative Weights)

> **Source:** Aaronson (2004, postselected learning); made constructive in arXiv:1711.01053 (Aaronson, Shadow Tomography, STOC 2018)
> **Tags:** #trick #shadow-tomography #multiplicative-weights #online-learning #quantum-learning

## What it does
Maintains a hypothesis state $\rho_t$ that converges to approximate agreement with an unknown $\rho$ on all target observables, updating by postselecting on measurements where the hypothesis currently fails. Converges in $O(\log D)$ iterations regardless of $M$.

## The trick

**Initialise:** $\rho_0 = I/D$ (maximally mixed state — maximum entropy prior).

**Update rule:** At each step $t$, find a measurement $E_j$ where $|\operatorname{Tr}(E_j \rho_t) - \operatorname{Tr}(E_j \rho)| \ge \varepsilon$. Update:
$$\rho_{t+1} \propto F_t \rho_t F_t^\dagger$$
where $F_t$ is a "filter" operator that postselects on the hypothesis being wrong — it applies $E_j$ (or its complement) and conditions on outcomes inconsistent with $\rho_t$'s prediction.

**Why it converges:** The update strictly increases the relative entropy $S(\rho \| \rho_t)$... wait, actually the right potential function is $\log \operatorname{tr}(\rho_t^{-1} \rho)$ or equivalently the **purity** of $\rho_t$ in the right basis. Each postselection on a failing measurement increases $-\log \operatorname{tr}(\rho_t^2)$ (or decreases the von Neumann entropy) by a constant. Since entropy is bounded below by $0$ and above by $\log D$, the number of updates is $T = O(\log D / \varepsilon)$.

More precisely: the multiplicative-weights potential $\Phi_t = \operatorname{tr}(\rho \rho_t^{-1})$ (or an approximation thereof) increases multiplicatively by a factor $\ge 1 + \Omega(\varepsilon^2)$ at each step, and is bounded above by $D^2 / \operatorname{tr}(\rho^2) \le D^2$, giving $T = O(\varepsilon^{-2} \log D)$ iterations.

**Constructive version:** In the original postselected learning theorem (Aaronson 2004), finding the failing measurement required a "teacher" who knew $\rho$. The contribution of shadow tomography is to replace this with [[Gentle Search via Quantum OR]], finding the failing measurement using $O(\log^4 M / \varepsilon^2)$ actual copies of $\rho$ without knowing it.

## When to reach for it
- Learning a quantum state's behaviour on many observables when you can only access copies of the state.
- Any quantum online learning problem where you can afford $O(\log D)$ rounds but not $O(D^2)$ samples.
- As the outer loop in algorithms that need to certify approximate consistency with all members of a large constraint set.

## Complexity
- **Iterations:** $T = O(\varepsilon^{-2} \log D)$
- **Per-iteration sample cost:** $O(\log^4 M / \varepsilon^2)$ (for the [[Gentle Search via Quantum OR]])
- **Total:** $\tilde{O}(\varepsilon^{-4} \log^4 M \cdot \log D)$ copies

The $\log D$ iteration count is the key fact: it means the complexity depends on dimension only logarithmically.

## Caveat
- **Computationally expensive:** Maintaining and updating $\rho_t$ classically requires storing a $D \times D$ matrix — exponential for $n$-qubit systems. Brandão et al. circumvent this by using Gibbs states and semidefinite programming, but the sample complexity is the same.
- **Joint measurements:** The update and search require joint measurements across $q = O(\varepsilon^{-2} \log\log D)$ copies. No single-copy analogue achieves the same iteration bound.
- **$\varepsilon$-degradation:** Each search step runs at a slightly smaller gap to maintain soundness, introducing extra polylog factors in the total copy count.

## Related notes
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Gentle Search via Quantum OR]]
- [[Quantum OR Subroutine]]
