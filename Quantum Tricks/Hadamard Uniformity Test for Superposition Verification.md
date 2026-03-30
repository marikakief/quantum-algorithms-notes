# Hadamard Uniformity Test for Superposition Verification

> **Source:** John Watrous, arXiv:cs/9901015
> **Tags:** #trick #Hadamard #uniformity #verification #interactive-proofs

## What it does
Tests whether a quantum register is in the uniform superposition over a finite field $\mathbb{F} = GF(2^k)$ by applying the Hadamard transform and checking for the all-zeros state.

## The trick
The uniform superposition over $\mathbb{F} = GF(2^k)$ is $|u\rangle = 2^{-k/2}\sum_{r \in \mathbb{F}} |r\rangle$. Under the Hadamard transform $H^{\otimes k}$:

$$H^{\otimes k} |u\rangle = |0^k\rangle$$

So to test uniformity: apply $H^{\otimes k}$, measure, accept if and only if the outcome is $|0^k\rangle$.

The test is exact for the pure uniform superposition. The useful property is what happens when the register is *entangled* with other registers:

- If the register is in state $\sum_r \alpha_r |r\rangle |\phi_r\rangle$ (entangled with an auxiliary system), the probability of seeing $|0^k\rangle$ is $|\sum_r \alpha_r |\phi_r\rangle / 2^{k/2}|^2$.
- This equals 1 only if $\alpha_r |\phi_r\rangle$ is independent of $r$ — i.e., the register is *not entangled* with anything and is in the uniform superposition.

In the context of [[Quantum Transcript Compression for Interactive Proofs|quantum transcript compression]], this detects "look-ahead cheating": if a prover based early responses on late random challenges, the late challenges become entangled with the early responses, and the Hadamard test on the late challenges fails.

## When to reach for it
- Verifying that a quantum register is in a known superposition (particularly the uniform one) without collapsing other useful quantum information.
- Detecting unwanted entanglement between a "should-be-uniform" register and other registers.
- As a building block in quantum protocols where one party needs to verify that another party's quantum state has a specific structure.
- More generally: testing any state that maps to a known computational basis state under a known unitary (apply the unitary, check the basis state).

## Complexity
One layer of Hadamard gates ($O(k)$ single-qubit gates) plus one computational-basis measurement. Essentially free.

## Caveat
The test is one-sided: passing guarantees the state was $|u\rangle$ (modulo measurement post-selection), but failure only tells you the state wasn't exactly uniform — it doesn't diagnose the deviation. The test probability is $|\langle 0^k | H^{\otimes k} | \psi \rangle|^2$, which for near-uniform states may be close to 1, so parallel repetition is needed for high-confidence detection of small deviations.

## Related notes
- [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes]]
- [[Quantum Transcript Compression for Interactive Proofs]]
