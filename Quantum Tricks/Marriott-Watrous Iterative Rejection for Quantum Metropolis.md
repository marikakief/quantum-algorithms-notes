
> **Source:** Temme, Osborne, Vollbrecht, Poulin, Verstraete, Nature 471, 87 (2011); technique from Marriott & Watrous, Computational Complexity 14, 122 (2005)
> **Tags:** #trick #Metropolis #rejection #binary-measurement #QMA

## What it does
Recovers a pre-measurement quantum state after a failed accept/reject test, without cloning. Failure probability decays exponentially with the number of iterations.

## The trick

After a rejected Metropolis step, you need to return to the input eigenstate $|\psi_i\rangle$. No-cloning prevents keeping a backup. But the accept/reject measurement splits the Hilbert space into a 2D subspace: $|\psi^+\rangle$ (accept) and $|\psi^-\rangle$ (reject), which are orthogonal.

Define two binary projective measurements:
- **$Q$:** Re-run the accept/reject circuit ($U$, measure ancilla, $U^\dagger$). Determines "would this state be accepted?"
- **$P$:** Check if the current state has energy $E_i$ (via phase estimation). Outcome $P_1$ means "yes, same energy."

Alternate $P$ and $Q$ measurements:

$$Q \to P \xrightarrow{P_0} Q \to P \xrightarrow{P_0} Q \to P \xrightarrow{P_1} \text{done!}$$

Stop when $P_1$ is obtained (energy matches original). By Jordan's 1875 lemma on pairs of subspaces, two non-commuting projectors can be simultaneously block-diagonalised into $2 \times 2$ blocks. Within each block, alternating the two measurements has a constant probability of converging. After $n$ iterations:

$$p_{\text{fail}}(n) \leq \frac{1}{2e(n+1)}$$

So $O(1/\varepsilon)$ rounds give failure probability $< \varepsilon$.

## When to reach for it
- Any quantum algorithm that needs to "undo" a measurement outcome
- Quantum Metropolis rejection (the original application)
- QMA amplification (the Marriott-Watrous context)
- Situations where you need to project back to a subspace after a failed test, without disturbing the state more than necessary

## Complexity
$O(1/\varepsilon)$ iterations of two binary measurements. Each iteration costs one phase estimation + one accept/reject circuit application.

## Caveat
The convergence rate constant depends on the overlap structure between the two projectors' subspaces. The $1/2e(n+1)$ bound is worst-case. In practice convergence is often faster (exponential in $n$ with a problem-dependent constant $< 1$).

## Related notes
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Coherent Metropolis Accept-Reject via Phase Estimation]]
