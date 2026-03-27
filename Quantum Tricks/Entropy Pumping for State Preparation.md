
> **Source:** Lloyd, Science 273(5278), 1073–1078 (1996)
> **Tags:** #trick #state-preparation #cooling #entropy

## What it does
Prepares an arbitrary quantum state from a mixed/unknown initial state by iteratively dumping entropy into a single ancilla that gets reset.

## The trick

Start with one variable ("the dump") in a known state — say, a cooled ion in its ground state. Apply a unitary that transfers entropy from the remaining variables into the dump. Reset the dump (re-cool it). Repeat.

Each cycle removes one bit of entropy from the system. After $O(N)$ cycles, the remaining $N$ variables can be driven to any desired pure state. The specific unitary at each step determines *which* state you converge to.

The same idea works in reverse for measurement: correlate the system with a single read-out variable via a controlled unitary, measure the read-out, repeat for confidence. Then move to the next bit of the measurement outcome. This gives accurate nondemolition measurements of arbitrary observables from a single noisy, destructive read-out capability.

## When to reach for it
- Preparing specific initial states on a quantum simulator when direct state preparation isn't available
- When you only have one "cold" qubit (one variable you can reliably reset)
- Algorithmic cooling protocols
- Quantum error correction ancilla recycling — same structural idea

## Complexity
- **Cycles:** $O(N)$ to prepare an $N$-qubit state from a maximally mixed state
- **Per cycle:** one unitary on the full system + one reset of the dump variable
- The unitary at each step is typically $O(2^N)$ to specify for an arbitrary target state, so this doesn't circumvent the cost of *finding* the right unitaries — it just says the physical protocol exists

## Caveat
The method tells you *that* arbitrary states can be prepared, but doesn't tell you how to efficiently find the right sequence of unitaries for a given target state. For specific physically motivated states (ground states, thermal states), the unitary sequence may or may not be efficiently computable. The hard part is usually figuring out what unitaries to apply, not the pumping protocol itself.

Also, each reset must be much faster than the system's natural decoherence time, or you lose coherence during the pumping cycles.

## Related notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Stinespring Dilation for Open-System Simulation]]
