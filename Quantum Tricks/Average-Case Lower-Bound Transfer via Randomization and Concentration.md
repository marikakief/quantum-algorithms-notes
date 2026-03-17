
> **Tags:** #trick #lower-bounds #oracle-model
> **Source:** arXiv:0908.4398

## What it does

Strengthens worst-case lower-bound constructions (which exhibit a single hard instance) to statements about typical instances, by randomizing over the hard instance family and showing that hard instances are abundant.

## The setting

The main lower bounds in Childs–Kothari use specific circulant Hamiltonians designed to encode hard problems (see [[Hardness Embedding via Circulant Hamiltonian Eigenphases]]). To show that the hardness is not fragile (i.e., not limited to a single pathological instance), the paper randomizes the construction:

- Randomly permute the roles of different basis states (permutation-invariant randomization).
- Randomly flip signs or phases in the oracle description.
- Use concentration inequalities to show that a random instance from the resulting distribution is hard with high probability.

By this argument, the lower bound transfers from "there exists a hard Hamiltonian" to "a random Hamiltonian from this distribution is hard with high probability," which is a stronger and more robust statement.

## When this matters

A worst-case lower bound applies to adversarial inputs, but in practice one might hope that "generic" Hamiltonians are easier. The randomized argument rules this out: even random instances drawn from a natural distribution are hard.

## Caveat

The hard distribution is still specially constructed (e.g., circulant or sign-randomized variants). It doesn't directly say that physically motivated Hamiltonians (Hubbard, chemistry) are hard in the entry-oracle model — those typically have additional structure that could be exploitable.

## Limitations of the approach

The paper is primarily a worst-case complexity result, and the average-case argument is a secondary strengthening. The paper's main contribution is ruling out efficient simulation (in poly$(\|H\|t, \log N)$ queries) for dense Hamiltonians in the entry-oracle model, not characterizing which physical Hamiltonians are hard.

## Related notes
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]
- [[Hardness Embedding via Circulant Hamiltonian Eigenphases]]
- [[Norm-Gap Obstruction between H and abs(H) in Oracle Simulation]]
