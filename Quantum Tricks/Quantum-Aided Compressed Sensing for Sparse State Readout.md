
> **Source:** Wiebe, Braun, Lloyd, arXiv:1204.5242 (2012)
> **Tags:** #trick #tomography #compressed-sensing #quantum-ML #sparse #readout

## What it does
Recovers a sparse classical description of a quantum state with cost $\tilde{O}(M'^2)$ state preparations rather than $O(M)$ full tomography, when the state has effective support on only $M'$ out of $M$ basis elements.

## The trick

Suppose you have a procedure that prepares $|\lambda\rangle = \sum_j \lambda_j |j\rangle$ but $\boldsymbol{\lambda}$ is approximately $M'$-sparse ($M' \ll M$). Full tomography costs $O(M)$ preparations and measurements. Instead:

1. **Support identification.** Prepare $|\lambda\rangle$ and measure in the computational basis $O(M')$ times. The heavy components ($|\lambda_j|^2$ large) show up frequently. This identifies the support $S$ of size $M'$ with high probability.

2. **Restriction.** Reconstruct the problem restricted to the $M'$-dimensional subspace indexed by $S$. Re-prepare $|\lambda_S\rangle$ (the $M'$-dimensional version).

3. **Compressed-sensing tomography.** Apply standard compressed-sensing quantum state tomography to $|\lambda_S\rangle$:
   - $O(M' \log^2 M')$ Pauli measurement settings.
   - $O(M'/\varepsilon^2)$ samples per setting.
   - Total: $O(M'^2 \log^2 M' / \varepsilon^2)$ state preparations.
   - Outputs a classical bit-string description of $\boldsymbol{\lambda}|_S$ to error $\varepsilon$.

The key saving: $M'^2$ instead of $M$ (or worse, $2^n$ for $n$-qubit states in general).

## When to reach for it

- The quantum algorithm outputs a state $|\lambda\rangle$ encoding the solution to a problem, but you need a classical description of the solution.
- You have reason to believe the solution is sparse or compressible (e.g., only a few basis functions matter in a fitting problem; only a few eigenmodes dominate).
- General quantum tomography is prohibitively expensive ($M$ too large), but $M'$ is manageable.

## Complexity

- Step 1: $O(M')$ preparations for support identification.
- Steps 2–3: $O(M'^2 \log^2 M'/\varepsilon^2)$ preparations for tomography.
- Total: $\tilde{O}(M'^2/\varepsilon^2)$ state preparations from the quantum subroutine, times the cost per preparation.

Compare to full tomography: $O(M/\varepsilon^2)$ — exponentially better when $M' = O(\mathrm{polylog}\, M)$.

## Caveat

- Requires the state to be *genuinely* sparse. If $M'$ grows with $N$ (even as $O(\sqrt{N})$), the advantage can erode.
- Support identification by random measurement works only if the heavy components are well-separated in magnitude. Near-flat states (e.g., all $|\lambda_j|^2 \sim 1/M$) require a different approach.
- Compressed-sensing tomography requires appropriate measurement bases (e.g., random Pauli settings); this may not always be physically convenient.
- The total cost still has $O(M'^2)$ dependence, so for a fitting problem where all $M$ basis functions are relevant, this gives no advantage over classical methods.

## Related notes
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — avoids tomography entirely by estimating observables directly; better when $M'$ is not small but $M$ is
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — PAC-style approach; learns state behaviour from measurements without full reconstruction
- [[Fat-Shattering Dimension of Quantum States via Random Access Codes]]
