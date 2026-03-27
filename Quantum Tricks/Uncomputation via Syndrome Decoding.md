
> **Source:** Jordan, Shutty, Wootters, Zalcman, Schmidhuber, King, Isakov, Khattar, Babbush, arXiv:2408.08292
> **Tags:** #trick #uncomputation #syndrome-decoding #reversible-computing #DQI

## What it does

Uncomputes an auxiliary quantum register by solving a classical decoding problem in superposition, enabling a clean state preparation that would otherwise require inverting an underdetermined linear map.

## The trick

In the DQI algorithm, after computing the syndrome $B^T \mathbf{y}$ into an ancilla register, we need to uncompute the register $|\mathbf{y}\rangle$. Since $B$ is non-square ($m > n$), knowing $B^T \mathbf{y}$ doesn't uniquely determine $\mathbf{y}$. But we also know $|\mathbf{y}| \le \ell$ (the Hamming weight is bounded by the polynomial degree).

The problem of recovering $\mathbf{y}$ from $B^T \mathbf{y}$ subject to a Hamming weight constraint is exactly syndrome decoding for the code $C^\perp = \ker(B^T)$. A reversible implementation of any efficient classical decoder solves this:

1. Run the decoder on input $B^T \mathbf{y}$ (in superposition), outputting the recovered $\hat{\mathbf{y}}$.
2. XOR $\hat{\mathbf{y}}$ into the $|\mathbf{y}\rangle$ register, zeroing it out.
3. Discard the zeroed register.

This works because within the Dicke-state superposition, each branch has $|\mathbf{y}| \le \ell$, so if the decoder handles $\ell$ errors, every branch decodes correctly.

## When to reach for it

- You have a quantum algorithm that computes $f(x)$ into an ancilla but needs to uncompute the intermediate variable $x$ knowing only $f(x)$ and a structural constraint on $x$.
- The map $f$ is linear and the constraint on $x$ is a Hamming weight bound — this is literally syndrome decoding.
- More generally, any setting where "uncomputation requires solving an inverse problem" and the inverse is tractable under known constraints.

## Complexity

Cost equals the reversible circuit size of the decoder. For Berlekamp-Massey on Reed-Solomon codes of block length $p$: $O(p^2 \log^2 p)$ arithmetic operations, giving $\sim 10^8$ Toffolis at $p = 521$. For belief propagation on LDPC codes: typically $O(m \cdot \text{iterations})$ with iterations $\sim O(\log m)$.

## Caveat

The decoder must be implemented as a reversible circuit, which can substantially increase the gate count versus an irreversible classical implementation (ancilla overhead for reversibility). If the decoder fails on some fraction of inputs, those branches remain entangled with garbage, reducing the fidelity of the final state (Theorem 10.1 quantifies the graceful degradation). The decoder must also be deterministic — randomized decoders require derandomization or seed registers.

## Related notes
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
- [[DQI Semicircle Law for Optimization-Decoding Duality]]
- [[QFT-Based Amplitude Shaping via Polynomial Evaluation]]
- [[Optimization-to-Decoding Reduction via Code Duality]]
- [[Dicke State Preparation via Inequality Testing]]
