# Product-Formula Error Representation Switching

> **Tags:** #trick #trotter #analysis
> **Source:** Childs, Su, Tran, Wiebe, Zhu, arXiv:1912.08854

## What it does

[[Order-Condition Cancellation in Product Formulas|product-formula error]] can be represented in three equivalent forms:
- **Additive:** $S(t) = e^{tH} + A(t)$  
- **Multiplicative:** $S(t) = e^{tH}(I + M(t))$  
- **Exponentiated (Floquet):** $S(t) = \mathcal{T}\exp\!\int_0^t (H + E(\tau))\,d\tau$

These are equivalent in the sense that $\|A(t)\| = O(t^{p+1})$ iff $\|M(t)\| = O(t^{p+1})$ iff $\|E(\tau)\| = O(\tau^p)$ for a $p$-th order formula (Theorem 9 in the paper).

## The trick

Choose whichever representation makes the target argument cleanest:
- Additive is natural for operator norm bounds.
- Multiplicative is natural when composing many segments (triangle inequality in the relative error).
- Exponentiated is natural for imaginary-time / Lindbladian / ODE analysis where an effective generator is more meaningful than an operator difference.

Switching between them during a proof avoids re-deriving the same estimates from scratch in each setting. The equivalence theorem means you get all three for the price of one.

## When to reach for it

- Real-time vs imaginary-time analysis of the same formula.
- Comparing segment-number bounds derived in different representations.
- Need sharper constants than BCH truncation gives.

## Caveat

Bookkeeping gets technical at high order — tracking which representation is in use matters. The error representations are equivalent asymptotically; the constants in the equivalences can matter for small $t$.

## Related Paper Notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
