# Source-Channel Duality for Protocol Design

> **Source:** Abeyesinghe, Devetak, Hayden & Winter, arXiv:quant-ph/0606225; originally identified by Devetak & Winter
> **Tags:** #trick #source-channel-duality #quantum-shannon-theory #protocol-design

## What it does

Converts a state-manipulation protocol (the "mother") into a channel-coding protocol (the "father") by a simple relabelling: replace the reference system $R$ with the channel environment $E$, and use the maximally-entangled-state trick to transfer operations from the reference to Alice's encoding.

## The trick

**Mother setting:** Alice and Bob share $(|\varphi\rangle^{ABR})^{\otimes n}$. The [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes|FQSW protocol]] decouples $A_2$ from $R$ by discarding $A_1$.

**Father setting:** Alice wants to send quantum information through a noisy channel $\mathcal{N}^{A' \to B}$ with Stinespring dilation $U_{\mathcal{N}}^{A' \to BE}$, using pre-shared entanglement $|\Phi\rangle^{A_3 B_3}$.

The correspondence:

| Mother (state) | Father (channel) |
|---|---|
| Reference $R$ | Environment $E$ |
| Shared state $\varphi^{AB}$ | Channel output state $\varphi^{ABE} = U_{\mathcal{N}}|\varphi\rangle^{AA'}$ |
| Alice discards $A_1$ | Alice sends $A_1$ through channel |
| Bob receives $A_1$ from Alice | Bob receives channel output |

The key step: the FQSW unitary $U^{B_3 R}$ acts on the reference, which is forbidden. But if the input is a maximally entangled state $|\Phi\rangle^{RA_0} \otimes |\Phi\rangle^{B_3 A_3}$, then $U^{B_3 R}$ on one half is equivalent to $(U^T)^{A_3 A_0}$ on Alice's half. So the mother's reference operation becomes Alice's encoding.

## When to reach for it

- Proving channel coding theorems (quantum capacity, entanglement-assisted capacity) from state manipulation results.
- Understanding why state and channel problems have formally similar capacity expressions.
- Any situation where a state-based protocol exists and you want the channel-based version (or vice versa).

## Complexity

No cost — this is a proof technique. The encoding and decoding operations inherit their complexity from the mother protocol.

## Caveat

The duality is formal: it works at the level of resource inequalities but doesn't always preserve operational details. For example, the mother's classical communication cost becomes an entanglement cost in the father. Two-way protocols don't dualize cleanly. Also, the maximally-entangled-state trick requires pre-shared entanglement, so the father protocol needs entanglement assistance.

## Related notes
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]]
- [[Unification by Resource Inequality Calculus]]
- [[Decoupling by Random Measurement]]
