# Adversary-as-Upper-Bound via Span Program Conversion

> **Source:** Childs, Kothari, Kovacs-Deak, Sundaram, Wang, arXiv:2210.06419 (TQC 2025); Špalek-Szegedy 2006; Reichardt 2009–2011
> **Tags:** #trick #query-complexity #adversary-method #span-programs #upper-bound #algorithm-design

## What it does

Uses the general (negative-weight) adversary bound not just as a lower bound tool but as an algorithm *construction* tool — the adversary quantity $\text{Adv}^\pm(f)$ is simultaneously a lower bound on $Q_2(f)$ and (up to constant factors) achievable by an explicit quantum algorithm.

## The trick

By the equivalence proved in [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]:
$$Q_2(f) = \Theta(\text{Adv}^\pm(f)) = \Theta(\text{SPC}(f))$$
where $\text{SPC}(f)$ is the span program complexity. The conversion goes both ways:

1. **Algorithm → adversary witness:** Given any $T$-query algorithm, extract an adversary matrix achieving $\Omega(T)$.
2. **Adversary witness → algorithm:** Given a span program $P$ for $f$ with complexity $w$, Reichardt's algorithm evaluates $f$ using $O(w)$ queries.

So to design a quantum algorithm for $f$, it suffices to exhibit a span program (or equivalently, an adversary witness) of small complexity — the algorithm comes for free.

**Why useful for composition:** Span programs compose nicely:
- $f = f_1 \text{ AND } f_2$: $\text{SPC}(f) \leq \sqrt{\text{SPC}(f_1)^2 + \text{SPC}(f_2)^2}$
- $f = f_1 \text{ OR } f_2$: similarly
- $f = $ OR of $a$ copies of $f_0$: $\text{SPC}(f) \leq \sqrt{a} \cdot \text{SPC}(f_0)$

This last rule is the engine of the [[Quantum Divide-and-Conquer Recurrence (sqrt-a Rule)|quantum divide-and-conquer speedup]].

## When to reach for it

- You want to prove both a lower bound and a matching upper bound in one shot.
- Your problem has a natural recursive or compositional structure where you can write $f$ as a Boolean combination of simpler functions with known span programs.
- You're designing a quantum algorithm and want to avoid constructing explicit quantum circuits — instead, argue algebraically about span program complexity.
- You're combining results from different papers that each give span programs for components, and you want to compose them.

## Complexity

The span program $\to$ algorithm conversion runs in $O(\text{SPC}(P))$ queries with $O(\text{SPC}(P)^2)$ ancilla qubits (in some formulations, sub-linear ancilla suffices). The conversion from adversary matrix to span program and back is polynomial-time classical computation.

For composed problems: if each of $a$ subproblems has span program complexity $w$, the OR of all $a$ has span program complexity $\sqrt{a} \cdot w$. Solving the resulting recurrence gives the quantum query complexity.

## Caveat

- The adversary-as-upper-bound only applies to the **general (negative-weight) adversary** $\text{Adv}^\pm$. The original positive-weight adversary $\text{Adv}$ is not tight for all functions.
- Span program complexity characterises query complexity, not circuit complexity. An additional step is needed to bound the number of gates.
- Composing span programs for the divide-and-conquer framework requires the subproblems to be structurally independent.

## Related notes

- [[Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025) — Paper Notes]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Weight Scheme Adversary Method]]
- [[Quantum Divide-and-Conquer Recurrence (sqrt-a Rule)]]
