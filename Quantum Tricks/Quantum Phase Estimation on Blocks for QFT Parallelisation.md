# Quantum Phase Estimation on Blocks for QFT Parallelisation

> **Source:** Kahanamoku-Meyer, Blue, Bergamaschi, Gidney, Chuang, arXiv:2505.00701
> **Tags:** #trick #QFT #circuit-depth #phase-estimation #parallelisation

## What it does

Breaks the serial dependency in the approximate QFT by using inverse QFT on already-transformed blocks to approximately recover block input values, enabling parallel phase rotations on neighbouring blocks.

## The trick

The standard approximate QFT on blocks of $m$ qubits has a serial dependency: to transform block $i$, you need the original input value $X_{i+1}$ of block $i+1$ for the inter-block phase rotation $\omega^{X_{i+1} Y_i / 2^m}$. But after transforming block $i+1$, the original $X_{i+1}$ is gone — it's been replaced by the Fourier-transformed state.

The trick: **quantum phase estimation can approximately recover $X_{i+1}$ from the transformed block.** After applying $\text{QFT}_{2^m}$ to block $i+1$, the state on that block is

$$[\Phi_x]_{i+1} = \sum_{Y_{i+1}} \omega^{(X_{i+1} + X_{i+2}/2^m) Y_{i+1}} |Y_{i+1}\rangle$$

Applying $\text{QFT}_{2^m}^\dagger$ performs quantum phase estimation and yields a superposition $|\widetilde{X}_{i+1}\rangle = \sum \alpha_{X'} |X'\rangle$ peaked on values $X'$ close to $X_{i+1} + X_{i+2}/2^m$.

For most values of $X_{i+1}$, the QPE distribution is tightly concentrated and using $|\widetilde{X}_{i+1}\rangle$ to control the phase rotation introduces only small error. The phase kickback to the block is also negligible when the approximation is good, so the state is approximately undisturbed and a final QFT returns it to the correct Fourier-basis output.

**Where it fails:** When $X_{i+1}$ is near $0$ or $2^m$, the QPE distribution wraps around modulo $2^m$ (the Lee metric distance $|X' - X_{i+1}|_{2^m}$ is small, but $X' - X_{i+1}$ in ordinary arithmetic is large). This "wraparound" produces a large phase error. These bad states form an $O(\varepsilon)$-fraction of the Hilbert space, making the overall construction an [[Optimistic Quantum Circuits|optimistic circuit]].

## When to reach for it

- Any circuit where the QFT's serial inter-block dependencies are the depth bottleneck.
- More generally: when a transformed register encodes its own input as a phase, QPE can approximately extract it and the result can be used to control operations on other registers.
- When the application tolerates rare large errors (or when the [[Worst-to-Average-Case Reduction for Optimistic Circuits|worst-to-average-case reduction]] can be applied).

## Complexity

The block QPE is just an $m$-qubit inverse QFT — same cost as the forward QFT on one block. For block size $m = O(\log(n/\varepsilon))$:

- Depth per block: $O(m) = O(\log(n/\varepsilon))$
- Total depth of optimistic QFT: $O(\log(n/\varepsilon))$ (three layers of parallel block operations)
- Gate range: $O(m) = O(\log(n/\varepsilon))$ in 1D

## Caveat

The trick depends on the QFT's product-state structure (Equation 15 in the paper): each block's output depends on a bounded number of neighbouring blocks. For unitaries without this local structure, block QPE would not recover the input.

The wraparound error is inherent to finite-register QPE and cannot be eliminated without long-range information flow. The states $|000\ldots0\rangle$ and $|111\ldots1\rangle$, where the QFT genuinely requires long-range propagation, are exactly the states where this trick fails. This is the fundamental reason why the construction is optimistic rather than exact.

## Related notes
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — source paper
- [[Optimistic Quantum Circuits]] — the framework that handles the wraparound error
- [[Worst-to-Average-Case Reduction for Optimistic Circuits]] — converts the optimistic QFT to a standard approximation
- [[Quantum Fourier Transform Circuit]] — the base QFT used inside blocks
- [[Iterative Phase Estimation (Kitaev)]] — the general QPE framework underlying block estimation
- [[Non-Uniform Gradient Estimation via Variable-Precision QFT]] — another use of QPE precision control
