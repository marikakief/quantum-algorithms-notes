# Recursive Amplitude Amplification with Approximate Reflections

> **Source:** Magniez, Nayak, Roland, Santha, SIAM J. Comput. 40(1), 2011; building on Høyer, Mosca, de Wolf (2003)
> **Tags:** #trick #amplitude-amplification #recursion #error-reduction #search

## What it does
Runs [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] with an *imperfect* reflection operator, eliminating the logarithmic overhead that a naive approach would incur.

## The trick

Standard Grover/amplitude amplification needs $T = O(1/\sqrt{\varepsilon})$ iterations of $\text{ref}(\pi) \cdot \text{ref}(\mu^\perp)$. If $\text{ref}(\pi)$ has error $\beta$, after $T$ iterations the total error is $\sim T\beta$. Setting $\beta < 1/T$ costs $O(\log T)$ per reflection — hence the log overhead.

The fix: replace iterative amplification with a *recursive* scheme.

$$A_0 = \text{Id}, \quad A_i = A_{i-1} \cdot R_i \cdot A_{i-1}^\dagger \cdot \text{ref}(\mu^\perp) \cdot A_{i-1}$$

where $R_i$ is an approximate reflection with precision $\beta_i = O(\gamma / i^2)$. Each level triples the rotation angle: $A_i$ rotates by $3^i \varphi$ where $\sin\varphi = \sqrt{p_M}$. After $t = \log_3(1/\varphi)$ levels, the state is close to $|\mu\rangle$.

Why it works: the error at level $i+1$ satisfies

$$e_{i+1} \leq 2\beta_{i+1} \bar{\varphi}_i + 3e_i$$

With $\beta_i = O(\gamma/i^2)$, the normalised errors $u_i = e_i / (\gamma \bar{\varphi}_i)$ satisfy $u_i = \frac{4}{3\gamma}\sum_{j=1}^{i} \beta_j \to 1/\pi$. The series $\sum \beta_j$ converges, so errors stay bounded by $\gamma$ throughout.

Total cost: $O(3^t \cdot (c_1 \log(1/\gamma) + c_2)) = O(\frac{1}{\sqrt{\varepsilon}} \cdot (\text{reflection cost} + \text{checking cost}))$ — the log factor is absorbed into the recursive structure.

## When to reach for it

- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|Walk-based search]] where the approximate reflection costs $O(\log(1/\beta)/\sqrt{\delta})$ per call — RAA avoids paying $\log(1/\sqrt{\varepsilon})$ on top
- Any amplitude amplification where one of the two reflections is only approximately available, and you want to match the $O(1/\sqrt{\varepsilon})$ query count of exact Grover
- Scenarios where the reflection quality degrades gracefully with precision parameter, and you can tune it per iteration

## Complexity
$O(1/\sqrt{\varepsilon})$ total checking operations and $O(1/\sqrt{\delta\varepsilon})$ total walk steps. The recursive structure adds at most a constant factor over exact amplitude amplification.

## Caveat
The recursion depth is $t = O(\log(1/\sqrt{\varepsilon}))$ levels, each using independent ancilla registers. Total ancilla count grows as $\sum_{i=1}^{t} s(\beta_i) = O(t \cdot \log(1/\Delta(P))) = O(\log(1/\sqrt{\varepsilon}) \cdot \log(1/\sqrt{\delta}))$ qubits. This is logarithmic, so not a practical concern, but worth noting.

The analysis assumes the checking reflection $\text{ref}(\mu^\perp)$ is exact. If both reflections are approximate, the technique can likely be extended but the MNRS paper doesn't prove this.

## Related notes
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Recursive Phase Estimation for Qubit-Efficient Readout]] — different recursive technique, similar flavour
