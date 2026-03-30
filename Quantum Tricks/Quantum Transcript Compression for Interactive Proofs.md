# Quantum Transcript Compression for Interactive Proofs

> **Source:** John Watrous, arXiv:cs/9901015
> **Tags:** #trick #interactive-proofs #QIP #PSPACE #parallelisation #complexity

## What it does
Compresses a multi-round classical interactive proof into a constant-round quantum interactive proof by encoding all possible transcripts in a single quantum message.

## The trick
Take a classical interactive proof with $N$ rounds, where in each round the prover sends a response and the verifier sends a random challenge. Instead of $N$ rounds:

1. The prover prepares a uniform superposition over all $2^{kN}$ possible challenge sequences $R = (r_1, \ldots, r_N) \in \mathbb{F}^N$, paired with the correct responses $C(R)$:
$$|\psi\rangle = 2^{-kN/2} \sum_R |R\rangle |C(R)\rangle$$

2. The prover sends this superposition to the verifier in one message.

3. The verifier picks a random "cut point" $u$, sends back the responses from position $u$ onward, and challenges the prover to "undo" its computation on those responses.

4. The verifier applies a [[Hadamard Uniformity Test for Superposition Verification|Hadamard uniformity test]] to check that the random-challenge registers after the cut point remain in uniform superposition.

**Why cheating is detectable:** A cheating prover must, at some round $k$, send an incorrect response polynomial. The incorrect response generically depends on the random challenges at rounds $> k$ (the prover "looked ahead"). This creates entanglement between the early responses and late challenges. When the verifier asks the prover to undo the late responses and tests the late challenges for uniformity, the entanglement prevents the challenges from returning to a uniform superposition.

The probability of detection is amplified by running $m$ parallel copies, giving exponentially small soundness error.

## When to reach for it
- Collapsing multi-round interactive proofs into constant-round protocols when quantum communication is available.
- Any setting where you want to verify that a party's computation at time $t$ didn't use information from time $t' > t$ — i.e., detecting temporal "look-ahead" using entanglement structure.
- As a conceptual tool: quantum mechanics enforces a kind of temporal honesty that classical protocols can't, because looking ahead creates entanglement that's detectable.

## Complexity
The honest prover needs exponential time to prepare the superposition (it must compute correct responses for all possible challenge sequences). The verifier runs in polynomial time. The protocol uses 2 messages (one quantum message each way) and the soundness error is exponentially small for polynomially many parallel copies.

## Caveat
The prover's preparation cost is exponential — this is fine for defining complexity classes (where the prover has unlimited power) but means the protocol isn't efficiently realisable. The technique is tied to the algebraic structure of the LFKN/Shen arithmetisation protocol; generalising to other interactive proof structures requires different analysis. Also only works against a single quantum prover — multi-prover settings introduce entanglement between provers that breaks the argument.

## Related notes
- [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes]]
- [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes]]
- [[Hadamard Uniformity Test for Superposition Verification]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
