# Extensive vs Intensive Error Targeting

> **Source:** Kivlichan, Gidney, Babbush et al., arXiv:1902.10673, Quantum **4**, 296 (2020)
> **Tags:** #trick #error-analysis #resource-estimation #Trotter #qubitization #condensed-phase #fault-tolerant

## What it does

Distinguishes two physically meaningful precision targets for condensed-phase quantum simulation, with dramatically different algorithmic consequences:

- **Intensive error** $\Delta E = \Theta(1)$: fixed absolute energy precision (e.g., chemical accuracy). Appropriate when the energy gap of interest doesn't scale with system size.
- **Extensive error** $\Delta E = \Theta(N)$: energy precision proportional to system size (e.g., energy per unit cell to fixed relative accuracy). Appropriate for bulk properties, equations of state, phase boundaries.

## The trick

For second-order Trotter with error norm $W$ and $N_r$ rotations per step, the total T-count scales as $\tilde{O}(N_r \sqrt{W} / \Delta E^{3/2})$. The $\Delta E^{3/2}$ denominator means that allowing extensive error ($\Delta E = \Theta(N)$) can reduce cost by $N^{3/2}$.

Concrete example for the Hubbard model:
- $W = O(N)$, $N_r = O(N)$
- Intensive: T-count $= O(N^{3/2})$
- Extensive: T-count $= O(N^{3/2}/N^{3/2}) = O(1)$

Compare [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]: T-count $= O(\lambda N / \Delta E)$, which gives $O(N)$ with extensive error for Hubbard — worse than Trotter's $O(1)$.

The choice of error target changes which algorithm wins. This is not a mathematical trick but a *problem formulation* insight: you should ask "what precision do I actually need?" before choosing your simulation method.

## When to reach for it

- Any resource estimation for condensed-phase or periodic systems
- Comparing Trotter vs LCU/qubitization methods — the winner depends on the error target
- Planning fault-tolerant quantum chemistry experiments on materials (not molecules, where intensive error is usually appropriate)

## Complexity

No additional circuit cost — this is purely about how you allocate your error budget. The savings come from allowing the total error to grow with system size, which reduces the number of Trotter steps needed.

## Caveat

Not all condensed-phase problems admit extensive error. If you care about a property determined by an intensive energy scale (e.g., the BCS gap in a superconductor, which is $\Theta(1)$), you need intensive precision regardless of system size. The paper carefully notes this and provides resource estimates for both regimes.

Also, the $O(1)$ Hubbard result holds only up to $N \sim 10^5$ orbitals — beyond that, the number of Trotter steps hits 1 and a different cost regime takes over. This limit is irrelevant for any foreseeable simulation.

## Related notes
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — source paper with full resource estimates for both regimes
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the competing approach that loses to Trotter under extensive error for Hubbard
- [[Trotterized Time Evolution for Chemistry]] — the Trotter framework that benefits from extensive error targeting
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization resource estimates for comparison
