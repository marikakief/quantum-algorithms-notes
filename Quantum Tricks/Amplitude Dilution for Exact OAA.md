
> **Tags:** #trick #amplitude-engineering #OAA #ancilla
> **Source:** Berry, Childs, Cleve, Kothari, Somma. arXiv:1312.1414, Lemma 3.4

## What it does
Reduces an arbitrary success amplitude $\sqrt{p} > 1/2$ to exactly $1/2$, enabling single-round exact [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]].

## The trick
Given $U|0^\mu\rangle|\psi\rangle = \sqrt{p}|0^\mu\rangle V|\psi\rangle + \sqrt{1-p}|\Phi^\perp\rangle$ with $p > 1/4$:

1. Add one ancilla qubit initialised to $|0\rangle$
2. Apply a controlled rotation $\Upsilon$: $|0\rangle \to \frac{1}{2\sqrt{p}}|0\rangle + \sqrt{1 - \frac{1}{4p}}|1\rangle$
3. The combined amplitude on $|0\rangle_{\text{anc}}|0^\mu\rangle V|\psi\rangle$ is now $\sqrt{p} \cdot \frac{1}{2\sqrt{p}} = \frac{1}{2}$

Now OAA with $\ell = 1$ gives exact amplification: $\sin(3\pi/6) = 1$.

## When to reach for it
- The natural [[Linear Combination of Unitaries (LCU)|LCU]] coefficient sum $s$ doesn't equal 2 (so amplitude isn't $1/2$)
- You know the exact value of $p$ (or a good bound) and want exact single-round OAA
- As an alternative to choosing segment length to force $s = 2$ ([[Taylor Series Truncation with ln2 Segmentation|ln 2 trick]])

## Complexity
One extra ancilla qubit + one controlled rotation. Negligible overhead.

## Caveat
Requires knowing $p$ (or a lower bound on it). If $p$ is state-dependent, this doesn't work — need fixed-point amplitude amplification instead ([[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]).

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
