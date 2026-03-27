
> **Source:** Yulong Dong, Lin Lin, and Yu Tong, arXiv:2204.05955 (PRX Quantum 2022)
> **Tags:** #trick #QSP #QET-U #eigenvalue-transformation #early-fault-tolerant

## What it does

Applies a polynomial function $f(H) = F(\cos(H/2))$ to a quantum state using only the **time evolution operator** $U = e^{-iH}$, **one ancilla qubit**, and **single-qubit rotations**. No block encoding, no multi-qubit control.

## The trick

Given an even real polynomial $F(x)$ of degree $d$ with $|F(x)| \leq 1$ on $[-1,1]$, find symmetric phase factors $\Phi_z = (\varphi_0, \varphi_1, \ldots, \varphi_1, \varphi_0) \in \mathbb{R}^{d+1}$ and build the circuit:

$$\mathcal{U}_{\Phi_z} = e^{i\varphi_0 X} U^\dagger\, e^{i\varphi_1 X}\, U\, e^{i\varphi_2 X} \cdots e^{i\varphi_2 X} U^\dagger\, e^{i\varphi_1 X}\, U\, e^{i\varphi_0 X}$$

Then:
$$(\langle 0| \otimes I_n)\, \mathcal{U}_{\Phi_z}\, (|0\rangle \otimes I_n) = F(\cos(H/2))$$

The circuit is almost identical to a repeated Hadamard test — just $d$ applications of $U$/$U^\dagger$ interleaved with $X$-rotations on the ancilla.

The key insight: the eigenvalue $\lambda_j$ of $H$ maps to $\cos(\lambda_j/2)$ in the QSP signal, and the invariant subspace $\text{span}\{|0\rangle|v_j\rangle, |1\rangle|v_j\rangle\}$ gives the $2 \times 2$ SU(2) structure needed for [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP/QSVT]].

## When to reach for it

- Ground-state energy estimation or ground-state preparation on **early fault-tolerant devices** where block encoding is too expensive.
- Any task requiring polynomial eigenvalue transformation of a Hamiltonian when $e^{-iH}$ is available (e.g., via [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Trotter]] decomposition).
- As a building block inside other QSP-based algorithms when the unitary input model is more natural than block encoding.
- Eigenvalue filtering, spectral projection, approximate sign function evaluation.

## Complexity

- **Queries to $U$:** $d$ (the polynomial degree)
- **Ancilla qubits:** 1
- **Additional gates:** $d+1$ single-qubit $X$-rotations
- **No multi-qubit control** (unless using the near-optimal variant with amplitude amplification)

For ground-state energy estimation: $d = \tilde{O}(\varepsilon^{-1}\log\gamma^{-1})$ per fuzzy bisection step, $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ total.

## Phase factor computation

The phase factors are found numerically via:
1. Construct optimal polynomial $F$ by convex optimization (min-max approximation to shifted sign function)
2. Find phase factors $\Phi_z$ via quasi-Newton optimization starting from $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$

Works up to degree $\sim 10000$ on a laptop. Implemented in [QSPPACK](https://github.com/qsppack/QSPPACK).

## Caveat

- Only implements **even** polynomials of $\cos(H/2)$. Odd polynomials need a different convention.
- Requires $H$ spectrum in $[\eta, \pi - \eta]$ for some $\eta > 0$ — the cosine map degenerates at $0$ and $\pi$.
- In general, still needs **controlled** $U$. Control-free variants exist for specific Hamiltonians with anti-commuting structure (see [[Control-Free QET-U via Anti-Commuting Pauli Strings]]).

## Related notes
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Binary Amplitude Estimation via Recursive QET-U]]
- [[Control-Free QET-U via Anti-Commuting Pauli Strings]]
