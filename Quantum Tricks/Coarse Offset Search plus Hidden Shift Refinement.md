# Coarse Offset Search plus Hidden Shift Refinement

> **Source:** Montanaro, arXiv:1408.1816
> **Tags:** #trick #pattern-matching #hidden-shift #amplitude-amplification #search

## What it does

Searches over coarse candidate locations, then uses hidden-shift recovery to refine a nearby guess to the exact match location.

## The trick

Instead of Grover-searching all offsets directly, define a bounded-error predicate `RoughCheck(t)`:

1. Build injectivised windows around offset $t$.
2. Run a hidden-shift algorithm to estimate the displacement $\ell$ between pattern and text windows.
3. Reject if $\ell$ falls outside the allowed tolerance range.
4. Verify $t+\ell$ using [[Standard Amplitude Amplification]] over mismatch positions.

If $t$ is within an $\epsilon m$-sized box of the true offset, `RoughCheck(t)` accepts. A Grover search over coarse offsets therefore costs $O((n/(\epsilon m))^{d/2})$ calls.

## When to reach for it

- Matching/alignment tasks where exact verification is expensive but a near offset can be refined by a group-shift algorithm.
- Problems with a hidden continuous/discrete displacement plus a large search space of possible starting locations.

## Complexity

For $d$ dimensions, the outer search contributes

$$
O\!\left((n/(\epsilon m))^{d/2}\right)
$$

calls to the refinement predicate. In Montanaro's application, $\epsilon$ is set by the tolerated error of the hidden-shift subroutine.

## Caveat

The verifier must reject false offsets reliably. If non-matches can differ in only one or two positions, verification costs $\Omega(m^{d/2})$ and the average-case advantage disappears.

## Related notes

- [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]
- [[Injectivise Pattern Matching by Adjacent Blocks]]
- [[Standard Amplitude Amplification]]
- [[Quantum Search on Bounded-Error Inputs (Høyer-Mosca-de Wolf 2003) — Paper Notes]]
