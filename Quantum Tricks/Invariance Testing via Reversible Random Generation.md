# Invariance Testing via Reversible Random Generation

> **Source:** Watrous, arXiv:cs/0009002 (2000)
> **Tags:** #trick #QMA #verification #group-theory #reversible-computation

## What it does

Tests whether a quantum state $|\psi\rangle$ is invariant under a group action (specifically, right multiplication by elements of a subgroup $H$), without knowing $H$'s structure — only needing the ability to generate nearly-uniform random elements of $H$.

## The trick

Given a state $|\psi\rangle$ on register $R$ and generators $g_1, \ldots, g_k$ of $H$:

1. **Generate in superposition:** Apply $F$ to ancilla register $S$ (initialised to $|0\rangle$), producing $\sum_{g \in H} \alpha_g |g\rangle|\text{garbage}(g)\rangle$ with $|\alpha_g|^2 \approx 1/|H|$.
2. **Multiply:** Use the group oracle to right-multiply $R$ by the element in $S$: $|\psi\rangle|g\rangle \mapsto |\psi g\rangle|g\rangle$.
3. **Uncompute:** Apply $F^\dagger$ to $S$.
4. **Measure $S$:** If the result is $|0\rangle$, proceed. Otherwise, reject.

**Why it works:** If $|\psi\rangle$ is invariant under $H$-multiplication (i.e., $|\psi\rangle = |H\rangle$), step 2 doesn't change the joint state, so $F^\dagger$ perfectly uncomputes $S$ back to $|0\rangle$. If $|\psi\rangle$ is NOT invariant, the multiplication entangles $R$ and $S$, preventing perfect uncomputation — $S$ has nonzero probability of not returning to $|0\rangle$.

**Projection bonus:** Conditioned on $S$ returning to $|0\rangle$, the state of $R$ is *projected* onto the $H$-invariant subspace. So even if the proof is slightly wrong, passing the test corrects it. This self-correcting property is why a single round of testing suffices to prepare $R$ for the subsequent non-membership test.

## When to reach for it

- Verifying quantum proofs in QMA protocols where the proof should be a group-invariant state
- Testing symmetry properties of quantum states without full tomography
- Any setting where you can generate random elements of a symmetry group reversibly
- The pattern generalises: to test any property that can be phrased as "invariant under some efficiently-sampleable transformation"

## Complexity

One call to $F$ (polynomial-time via Babai's theorem), one group oracle call, one call to $F^\dagger$, and one measurement. Total cost: polynomial in $n$ per test.

## Caveat

The generation procedure $F$ produces a *nearly*-uniform superposition (each $|\alpha_g|^2$ deviates from $1/|H|$ by at most $2^{-2n}$), which introduces exponentially small error in the test. This is handled by Watrous's analysis but means the test is not perfectly projective — there's $O(2^{-2n})$ leakage even for invariant states.

## Related notes
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]]
- [[Uniform Superposition as Quantum Proof]]
- [[Coset Distinguishing via Controlled Multiplication]]
- [[Subgroup State Preparation via Subnormal Chain]]
