# Negative Conditional Entropy as Entanglement Gain

> **Source:** Horodecki, Oppenheim & Winter, arXiv:quant-ph/0512247
> **Tags:** #trick #conditional-entropy #entanglement #quantum-shannon-theory #state-merging

## What it does

Gives operational meaning to negative quantum conditional entropy: when $S(A|B) < 0$, a state transfer protocol not only succeeds for free (via LOCC alone) but also *generates* $|S(A|B)|$ EPR pairs per copy.

## The trick

For a bipartite state $\rho_{AB}$ with $S(A|B) = S(AB) - S(B) < 0$, the [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes|state merging]] protocol transfers Alice's share to Bob and produces entanglement at rate $-S(A|B)$ ebits per copy.

Physically: when $S(B) > S(AB)$, Bob already "knows more" than the total system (Schrödinger's observation about entangled states). Alice's partial information is negative — sending it to Bob actually reduces his uncertainty, leaving surplus entanglement.

This resolves the puzzle of negative coherent information $I(A\rangle B) = S(B) - S(AB) = -S(A|B)$: it's the rate of entanglement *gained* during state merging.

## When to reach for it

- Analysing protocols where entanglement is generated as a byproduct of state transfer.
- Understanding the coherent information as an entanglement rate (positive $\to$ gain, negative $\to$ cost).
- Distributed quantum compression: if one party's conditional entropy is negative, they can over-compress below zero rate, contributing net entanglement to the protocol.
- Quantum multiple access channels: a sender with negative rate is *investing* entanglement to help the other sender achieve maximum throughput.

## Complexity

No additional cost beyond the state merging protocol itself. The entanglement gain comes for free from the LOCC protocol.

## Caveat

Only meaningful asymptotically (many copies). The one-shot analogue uses smooth min-entropy, which can differ from the von Neumann conditional entropy. Also, "negative information" is a property of the protocol/task, not the state alone — the same state might have positive or negative partial information depending on which system is being transferred.

## Related notes
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]]
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]]
- [[Decoupling by Random Measurement]]
