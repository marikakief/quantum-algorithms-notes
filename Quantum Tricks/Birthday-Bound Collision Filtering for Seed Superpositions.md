
> **Tags:** #trick #probabilistic-design #state-preparation #collision-filtering
> **Source:** npj Quantum Information 2018 (s41534-018-0071-5)

## What it does
Ensures high-probability preparation of repetition-free index strings by enlarging seed domain so collisions are rare.

## The trick
[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] seed as uniform superposition over strings of length $\eta$ drawn from $\{0,\dots,f(\eta)-1\}$. Reject strings with repeated entries after sorting.

Choose
$$f(\eta) \ge \eta^2$$
then success probability (collision-free) is bounded below by a constant (> 1/2 in the paper).

This is the quantum version of birthday-paradox engineering: spend a modest larger domain to make rejection sampling cheap.

## When to use
- Coherent preparation of distinct indices/subsets
- Any algorithm needing "without replacement" samples in superposition
- Seed generation before permutation/symmetrization routines

## Caveat
Requires postselection/restart. If restarts are expensive, combine with amplitude amplification.
