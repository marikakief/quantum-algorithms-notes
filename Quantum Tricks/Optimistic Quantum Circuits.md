# Optimistic Quantum Circuits

> **Source:** Kahanamoku-Meyer, Blue, Bergamaschi, Gidney, Chuang, arXiv:2505.00701
> **Tags:** #trick #circuit-design #approximation #Frobenius-norm #framework

## What it does

Formalises circuits that approximate a unitary well on most inputs but allow large error on a small fraction of the Hilbert space, and shows when this is sufficient.

## The trick

Define a circuit $\widetilde{U}$ as an **optimistic circuit** for $U$ with error $\varepsilon$ if

$$\frac{\|\widetilde{U} - U\|_F^2}{\dim \mathcal{H}} \leq \varepsilon$$

This is equivalent to: for any complete basis $\{|\phi_i\rangle\}$,

$$\frac{1}{\dim \mathcal{H}} \sum_i \|\widetilde{U}|\phi_i\rangle - U|\phi_i\rangle\|^2 < \varepsilon$$

The definition is **basis-independent** because it uses the Frobenius norm (sum of squared error vector lengths over any basis). This distinguishes it from classical average-case complexity, which depends on the input distribution.

**Key lemma (bad subspace bound):** If $\widetilde{U}$ is optimistic with parameter $\varepsilon$, then the subspace of states with per-state error $\geq \mu$ has dimension at most $(\varepsilon / \mu) \cdot \dim \mathcal{H}$. In particular, at most an $O(\varepsilon)$ fraction of basis states have $O(1)$ error.

**When optimistic circuits suffice as drop-in replacements:** If the algorithm feeds effectively random (or random-looking) inputs to the subroutine, the optimistic circuit's average-case guarantee is directly applicable. This is shown explicitly for [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: the intermediate values in modular exponentiation are random elements of the multiplicative group mod $N$, so the optimistic QFT's error is small in expectation and factoring succeeds with probability $\geq O(1) \cdot (1 - O(n\sqrt{\varepsilon}))$.

When drop-in replacement isn't sufficient, the [[Worst-to-Average-Case Reduction for Optimistic Circuits|worst-to-average-case reduction]] converts any optimistic circuit into a standard approximate circuit.

## When to reach for it

- Any quantum circuit for a "semiclassical" operation (classical function applied to a superposition) where the hard cases are rare — e.g., carry propagation in adders, the [[Quantum Fourier Transform Circuit|QFT]], modular inversion via the Euclidean algorithm.
- When you need simultaneous improvements in depth, ancilla count, and locality that are provably impossible for worst-case approximate circuits.
- As a design methodology: build an optimistic version first (often much simpler), then apply the reduction if worst-case guarantees are needed.

## Complexity

The optimistic circuit itself can be dramatically cheaper than a worst-case approximation. For the QFT:
- Optimistic: depth $O(\log(n/\varepsilon))$, 0 ancillae, nearest-neighbour in 1D
- Best prior worst-case: depth $O(\log n)$, $O(n)$ ancillae, long-range or measurement-based

The reduction adds the cost of implementing two elements of a unitary 1-design (one before, one after the optimistic circuit). For the QFT, this means one integer addition ($O(n)$ gates) and one phase gradient ($O(n)$ single-qubit gates).

## Caveat

The $O(\varepsilon)$-fraction bad subspace is small but not empty. For algorithms where the input to the subroutine is adversarially chosen or correlated with the bad subspace, the optimistic circuit cannot be used directly. The reduction handles this case but adds ancillae and potentially long-range gates.

The Frobenius-norm error $\varepsilon$ implies diamond-distance error $O(\sqrt{\varepsilon})$ when viewed as a channel. For applications requiring diamond-distance guarantees, set the optimistic error parameter to $\varepsilon' = \varepsilon^2$.

## Related notes
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — source paper
- [[Worst-to-Average-Case Reduction for Optimistic Circuits]] — the companion reduction technique
- [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] — the trick that makes the optimistic QFT work
- [[Quantum Fourier Transform Circuit]] — the target unitary for the main application
- [[Frobenius-Norm Average-Case Error Bound]] — related error metric used in randomized product formulas
