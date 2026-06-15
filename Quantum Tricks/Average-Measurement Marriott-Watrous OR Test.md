# Average-Measurement Marriott-Watrous OR Test

> **Source:** Harrow--Lin--Montanaro, arXiv:1607.03236
> **Tags:** #trick #sequential-measurements #Marriott-Watrous #property-testing #QMA

## What it does

Implements an OR over many noncommuting measurements without simply measuring them one after another.

## The trick

Given projectors $\Lambda_1,\ldots,\Lambda_n$, form the average POVM element

$$
\Lambda=\frac1n\sum_{j=1}^n\Lambda_j.
$$

Then apply a simplified Marriott--Watrous loop to a decomposition

$$
\Lambda\otimes |0\rangle\langle0|^{\otimes m}=\Delta\Pi\Delta.
$$

Alternate measuring $\Pi$ and the ancilla reset projector $\Delta$. Accept if $\Pi$ accepts or if $\Delta$ fails.

The acceptance probability after $N$ rounds obeys

$$
(1-e^{-1})\operatorname{tr}(P_{\ge 1/(2N)}\rho)
\le p_{\rm acc}(N)
\le 2N\operatorname{tr}(\Lambda\rho).
$$

## When to reach for it

- You need to test whether one of many measurements would accept a state.
- The measurements do not commute and naive sequential measurement can disturb the state.
- You can implement the averaged POVM element coherently.

## Caveat

The yes case is easiest when some measurement accepts with probability near 1. For weaker gaps, amplify individual tests first, often by tensor powers.

## Related notes

- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]
- [[Gentle Measurement Lemma]]
- [[Marriott-Watrous In-Place Amplification]]
