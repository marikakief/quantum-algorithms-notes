# Black-Box to Communication Reduction

> **Source:** Buhrman, Cleve, Wigderson, arXiv:quant-ph/9802040
> **Tags:** #trick #communication-complexity #query-complexity #simulation

## What it does
Converts any quantum oracle algorithm into a quantum communication protocol for a related two-party problem, or vice versa.

## The trick

Given a quantum algorithm that computes $F(f)$ using $t$ queries to $f : \{0,1\}^n \to \{0,1\}$, and a combining function $L : \{0,1\}^2 \to \{0,1\}$, construct a protocol where Alice (holding $g$) and Bob (holding $h$) compute $F(L(g,h))$.

For each oracle call in the algorithm, Alice and Bob collaborate:

1. Alice computes $g(x)$ onto an ancilla (in superposition over all $x$)
2. Alice sends the $n+2$ qubits to Bob
3. Bob applies $L(g(x), h(x))$ and XORs the result onto the target register
4. Bob sends the qubits back
5. Alice uncomputes her ancilla

Each simulated oracle call costs $2(n+2)$ qubits of communication. The quantum parallelism means the entire superposition is handled in one round-trip — there's no need to communicate for each branch separately.

**Reverse direction:** Communication lower bounds on the induced problem give query lower bounds on the original algorithm. For instance, $Q(\mathrm{IP}) \in \Omega(N)$ implies $T(\mathrm{PARITY}) \in \Omega(2^n/n)$ by setting $L = $ AND.

## When to reach for it

- Proving quantum communication separations: plug in any known quantum algorithm (Grover, Deutsch-Jozsa, etc.) to get a quantum protocol, then compare with classical lower bounds.
- Proving quantum query lower bounds: take a known communication lower bound and run the reduction backwards.
- Any setting where you need to convert between oracle access and distributed computation.

## Complexity

$t$ oracle queries → $O(tn)$ qubits of communication. Success probability is preserved exactly.

## Caveat

For total functions, the polynomial method (Beals et al. 1998) shows $T$-query quantum algorithms have classical $O(T^6)$ counterparts. So this approach can never yield more than a sixth-root separation for total functions. The exponential gaps require partial functions or weaker models (SMP, exact/zero-error).

## Related notes
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]]
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]]
- [[Approximate Oracle via Compute-CNOT-Uncompute]]
- [[Nested Grover Search for Bounded-Depth Predicates]]
