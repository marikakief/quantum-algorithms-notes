# Unification by Resource Inequality Calculus

> **Source:** Abeyesinghe, Devetak, Hayden & Winter, arXiv:quant-ph/0606225; formalism from Devetak, Harrow & Winter (2004/2008)
> **Tags:** #trick #resource-inequality #quantum-shannon-theory #protocol-design

## What it does

Treats quantum information resources (qubits of communication $[q \to q]$, shared entanglement $[qq]$, classical communication $[c \to c]$, noisy states $\langle \varphi \rangle$, noisy channels $\langle \mathcal{N} \rangle$) as algebraic objects in inequalities, allowing new protocols to be derived by combining known ones.

## The trick

Write any protocol as a resource inequality, e.g.:

$$\langle \varphi^{AB} \rangle + \tfrac{1}{2}I(A;R) [q \to q] \geq \tfrac{1}{2}I(A;B) [qq] + \langle \mathrm{id}^{A \to \hat{B}} \rangle$$

Then apply "Rule I" (coherent communication): each bit of classical communication $[c \to c]$ can be made coherent, converting to $\frac{1}{2}[q \to q] - \frac{1}{2}[qq]$ (a "cobit"). This works because sending the classical communication used in teleportation as coherent qubits recycles it into entanglement.

Combine protocols by:
- **Substitution:** feed the output of one protocol into the input of another
- **Teleportation:** convert $[q \to q]$ to $[qq] + 2[c \to c]$
- **Superdense coding:** convert $[qq] + [q \to q]$ to $2[c \to c]$
- **Time reversal:** run a protocol backwards (mother → reverse Shannon theorem)

## When to reach for it

- Deriving new quantum Shannon theorems from known ones without reproving from scratch.
- Checking whether a proposed rate region is achievable by composing existing protocols.
- Understanding the relationships between protocols (e.g., why the mother generates the quantum channel capacity as a special case).

## Complexity

Zero — this is a proof technique, not a computational procedure. The resource inequalities preserve asymptotic rates and the error vanishes by standard composition arguments.

## Caveat

The formalism is asymptotic. One-shot compositions are messier — errors accumulate, and the algebraic cleanness breaks down. Also, Rule I (coherent communication) requires careful handling; not every use of classical communication can be made coherent (it must be "decoupled" from the quantum state).

## Related notes
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]] — primary demonstration
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] — state merging as a child protocol
- [[Source-Channel Duality for Protocol Design]]
