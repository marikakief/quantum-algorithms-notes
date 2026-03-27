
> **Source:** Brassard, Høyer, Mosca & Tapp, arXiv:quant-ph/0005055 (2002); generalizing [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]]
> **Tags:** #trick #amplitude-amplification #search #fundamental

## What it does

Given a quantum algorithm $A$ that produces a good state with probability $a = \sin^2\theta_a$, boosts the success probability to near 1 using $O(1/\sqrt{a})$ applications of $A$ and $A^{-1}$.

## The operator

$$
Q = -A S_0 A^{-1} S_\chi
$$

where $S_\chi$ flips the sign of good states and $S_0$ flips the sign of $|0\rangle$. Equivalently, $Q$ is the product of two reflections: one about the bad subspace (via $S_\chi$) and one about $A|0\rangle$ (via $-AS_0 A^{-1}$).

## The 2D subspace

$Q$ acts on $\mathrm{span}\{|\Psi_1\rangle, |\Psi_0\rangle\}$ as a rotation by $2\theta_a$:

$$
Q^j|\Psi\rangle = \sin((2j+1)\theta_a)|\Psi_1'\rangle + \cos((2j+1)\theta_a)|\Psi_0'\rangle
$$

After $m = \lfloor\pi/(4\theta_a)\rfloor \approx O(1/\sqrt{a})$ iterations, success probability $\geq 1 - a$.

## When to reach for it

- Any quantum algorithm whose output needs checking: run $A$, check with $\chi$, amplify
- Boosting postselection probability in [[Linear Combination of Unitaries (LCU)|LCU]] (though [[Oblivious Amplitude Amplification (Robust)|oblivious AA]] is usually better there)
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]: boosts $1/\kappa^2$ success to $O(1)$ with $O(\kappa)$ rounds
- Search with classical heuristics: any expected-time-$T$ classical search becomes $O(\sqrt{T})$

## Complexity

$O(1/\sqrt{a})$ applications of $A$, $A^{-1}$, and $S_\chi$. Optimal — matches the $\Omega(\sqrt{N/t})$ lower bound for search.

## Caveat

Requires $A$ to be **measurement-free** (no intermediate measurements). Also requires $A^{-1}$ — if $A$ is irreversible or has garbage, this won't work (see [[Reversible Computation via Compute-Copy-Uncompute]]). If $a$ is unknown, use the exponential search strategy (Theorem 3) or [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]].

## Related notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the original algorithm; amplitude amplification generalises it
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Inversion About the Mean]] — the core reflection primitive from Grover
- [[Oblivious Amplitude Amplification (Robust)]] — the input-oblivious generalization
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[QSVT Meta-Template]] — amplitude amplification as a special case of singular value transformation
