# Worst-to-Average-Case Reduction for Optimistic Circuits

> **Source:** Kahanamoku-Meyer, Blue, Bergamaschi, Gidney, Chuang, arXiv:2505.00701
> **Tags:** #trick #circuit-design #approximation #1-design #randomization

## What it does

Converts any [[Optimistic Quantum Circuits|optimistic quantum circuit]] (small error on most inputs) into a standard approximate circuit (small error on all inputs), by sandwiching between random unitaries drawn from a 1-design.

## The trick

Given an optimistic circuit $\widetilde{U}$ for $U$ with error $\varepsilon$:

1. Draw $V$ uniformly from a unitary 1-design $d$ on $\mathcal{H}$.
2. Compute $\hat{V}^\dagger = U V^\dagger U^\dagger$.
3. Apply $\hat{V}^\dagger \widetilde{U} V$ to the input.

**Why it works:** $V$ maps any input state to an effectively random state (a state 1-design), where the optimistic circuit has low expected error. The operator $\hat{V}^\dagger$ undoes the randomisation: $\hat{V}^\dagger U V = U$ exactly, so the only error comes from $\widetilde{U} \neq U$. By the 1-design property:

$$\mathbb{E}_{V \sim d}\left[\|\hat{V}^\dagger \widetilde{U} V |\psi\rangle - U|\psi\rangle\|^2\right] \leq \varepsilon$$

for any state $|\psi\rangle$.

**Derandomisation via purification:** If the 1-design is a uniform distribution over $k$ unitaries $\{V_i\}$, define $V' = \sum_i |i\rangle\langle i| \otimes V_i$. Then $\hat{V}'^\dagger (\mathbb{I} \otimes \widetilde{U}) V'$ applied to $\frac{1}{\sqrt{k}} \sum_i |i\rangle \otimes |\psi\rangle$ achieves error $\leq \varepsilon$ deterministically. The control register has $\log_2 k$ qubits.

**For the QFT:** The Weyl–Heisenberg group $V(r_1, r_2) = X_{2^n}^{r_1} Z_{2^n}^{r_2}$ forms a 1-design. The QFT swaps $X$ and $Z$, so the conjugated operator is simply $\hat{V}^\dagger(r_1, r_2) = V(r_2, -r_1)$ — same structure, arguments swapped. This makes both $V$ and $\hat{V}^\dagger$ cheap to implement: each is an integer addition plus a phase gradient.

## When to reach for it

- When you have an optimistic circuit but need guaranteed worst-case error bounds.
- When the target unitary has a natural 1-design where both $V$ and $\hat{V}^\dagger = U V^\dagger U^\dagger$ are efficiently implementable. The QFT is the canonical example because it maps the Weyl–Heisenberg group to itself.
- As a general framework: design the cheap optimistic circuit first, then upgrade to worst-case via this reduction.

## Complexity

The reduction adds the cost of implementing $V$ and $\hat{V}^\dagger$ (two 1-design elements). For the QFT:
- **Randomized:** one classical-quantum addition ($O(n)$ gates, $O(n/\log n)$ ancillae) + one phase gradient ($O(n)$ single-qubit gates). Total overhead: $O(n)$ gates, $O(n/\log n)$ ancillae.
- **Derandomized:** controlled quantum-quantum addition (Takahashi–Tani–Kunihiro 2009) + controlled phase gradient. Adds $2n$ qubits for the control register; total $3n + O(n/\log(n/\varepsilon))$ qubits.

## Caveat

The 1-design must satisfy two conditions: (1) both $V$ and $\hat{V}^\dagger$ must have efficient circuits, and (2) the 1-design must be large enough to achieve the desired error bound. For the QFT these are easily satisfied, but for arbitrary unitaries $U$ the conjugated operator $\hat{V}^\dagger = U V^\dagger U^\dagger$ may not have an efficient circuit even when $V$ does.

The randomized version requires classical randomness and gives expected error bounds. The derandomized version is deterministic but costs $2n$ extra qubits (the control register for the purification of the Weyl–Heisenberg group).

## Related notes
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — source paper
- [[Optimistic Quantum Circuits]] — the framework this reduction converts from
- [[Quantum Fourier Transform Circuit]] — the main application target
- [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] — produces the optimistic QFT that the reduction upgrades
- [[Phase Gradient State for Controlled Rotations]] — the phase gradient rotation $Z_{2^n}^{r_2}$ used in the Weyl–Heisenberg 1-design
