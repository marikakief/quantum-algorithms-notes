
> **Source:** Buhrman, Cleve, Watrous & de Wolf, arXiv:quant-ph/0102001 (2001), Figure 1
> **Tags:** #trick #state-comparison #swap-test #inner-product #fundamental

## What it does

Given one copy each of unknown states $|\phi\rangle$ and $|\psi\rangle$, estimates $|\langle\phi|\psi\rangle|^2$ with a single measurement. The simplest quantum test for state equality.

## The circuit

$$
|0\rangle \xrightarrow{H} \frac{|0\rangle + |1\rangle}{\sqrt{2}} \xrightarrow{c\text{-SWAP}} \xrightarrow{H} \text{measure}
$$

Applied to $|0\rangle|\phi\rangle|\psi\rangle$:

1. Hadamard on control: $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)|\phi\rangle|\psi\rangle$
2. Controlled-SWAP: $\frac{1}{\sqrt{2}}(|0\rangle|\phi\rangle|\psi\rangle + |1\rangle|\psi\rangle|\phi\rangle)$
3. Hadamard on control: $\frac{1}{2}|0\rangle(|\phi\rangle|\psi\rangle + |\psi\rangle|\phi\rangle) + \frac{1}{2}|1\rangle(|\phi\rangle|\psi\rangle - |\psi\rangle|\phi\rangle)$
4. Measure control:

$$
\Pr[|0\rangle] = \frac{1 + |\langle\phi|\psi\rangle|^2}{2}, \qquad \Pr[|1\rangle] = \frac{1 - |\langle\phi|\psi\rangle|^2}{2}
$$

**One-sided error:** If $|\phi\rangle = |\psi\rangle$, always outputs $|0\rangle$. If $|\phi\rangle \neq |\psi\rangle$, outputs $|1\rangle$ with probability $(1 - |\langle\phi|\psi\rangle|^2)/2$.

## When to reach for it

- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes|Quantum fingerprinting]]: the referee's comparison protocol
- Inner product estimation between unknown states
- State certification: verify that a prepared state matches a reference
- As a subroutine in quantum machine learning (kernel estimation)
- Entanglement testing and state discrimination

## Complexity

One controlled-SWAP gate + two Hadamards. Repeat $k$ times (with $k$ copies of each state) for error probability $\leq (1/2 + \delta^2/2)^k$ when $|\langle\phi|\psi\rangle| \leq \delta$.

## Caveat

Destructive: consumes both states. Can't be used to *find* $|\langle\phi|\psi\rangle|$ from a single shot — only distinguishes "equal" from "not equal" with bounded error. For precision $\epsilon$ in $|\langle\phi|\psi\rangle|^2$, need $O(1/\epsilon^2)$ copies (standard sampling).

The controlled-SWAP on $d$-dimensional states requires $O(\log d)$ gates, not $O(1)$.

## Related notes

- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]]
- [[Quantum Fingerprinting via Error-Correcting Codes]]
- [[Density Matrix Exponentiation via Partial Swap]] — uses the (uncontrolled) swap operator for a different purpose
- [[Phase Kickback from Eigenstate Ancilla]] — similar Hadamard-control-Hadamard pattern
