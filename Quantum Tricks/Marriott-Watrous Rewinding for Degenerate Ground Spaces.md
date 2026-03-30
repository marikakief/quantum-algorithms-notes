# Marriott-Watrous Rewinding for Degenerate Ground Spaces

> **Source:** Schwarz, Cubitt, Temme, Verstraete, Perez-Garcia, arXiv:1211.4050
> **Tags:** #trick #Marriott-Watrous #state-preparation #topological-order #Jordan-lemma

## What it does
Extends the [[Jordan's Lemma Subspace Decomposition for Alternating Measurements|Marriott-Watrous rewinding technique]] from problems with unique ground states to those with degenerate ground state subspaces, enabling efficient state preparation for topologically ordered systems.

## The trick
The standard Marriott-Watrous technique alternates projective measurements $\{P, P^\perp\}$ and $\{Q, Q^\perp\}$ to transition from the ground state of one Hamiltonian to the ground state of the next. By Jordan's lemma, the two projectors decompose into 1D and 2D blocks, and the back-and-forth measurements form a Markov chain with the desired state as the absorbing state. For unique ground states, this converges rapidly.

For **degenerate** ground spaces (dimension $> 1$), the naive approach fails: $P$ and $Q$ project onto multi-dimensional subspaces, and a failed measurement could land in a state with zero forward-transition probability. The condition number $\kappa$ on the full space is infinite (because the PEPS tensors have zero eigenvalues outside the symmetric subspace).

The fix: restrict the condition number to the **G-symmetric subspace** $S_G$. If the PEPS is G-injective, its ground states all lie in $S_G$, and the minimum overlap between successive ground spaces satisfies:

$$d_{\min} \geq \kappa(A|_{S_G})^{-2} > 0$$

This bound ensures that *every* state in the current ground space has nonzero probability of transitioning forward, so the Markov chain still has the desired absorbing structure.

Failure probability after $m$ alternating measurements: $p_{\text{fail}}(m) < 1/(2\,d_{\min}\,m)$.

## When to reach for it
- Preparing topologically ordered states (toric code, quantum doubles, RVB states) on a quantum computer
- Any state preparation problem where you need to project through a sequence of frustration-free Hamiltonians with degenerate ground spaces
- Extending QMA amplification arguments to settings with degenerate witness subspaces

## Complexity
$O(\kappa_G^2 / \varepsilon)$ measurement repetitions per site, where $\kappa_G$ is the condition number on $S_G$ and $\varepsilon$ is the target error probability. Each measurement costs $\tilde{O}(N^2/\Delta)$ via quantum phase estimation, where $\Delta$ is the spectral gap.

## Caveat
Requires that the spectral gap $\Delta$ of the parent Hamiltonians doesn't close — if it does, each measurement becomes exponentially expensive. The gap assumption is model-dependent and not proven in general. Also, the technique prepares *some* state in the ground space, without control over which topological sector.

## Related notes
- [[Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) — Paper Notes]]
- [[Jordan's Lemma Subspace Decomposition for Alternating Measurements]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
