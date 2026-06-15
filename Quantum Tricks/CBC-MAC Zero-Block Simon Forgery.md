# CBC-MAC Zero-Block Simon Forgery

> **Source:** Santoli--Schaffner, arXiv:1603.07856
> **Tags:** #trick #Simon #CBC-MAC #quantum-cryptanalysis #symmetric-key

## What it does

Uses Simon’s algorithm to learn CBC chaining-state XORs from zero-block queries, then assembles an unqueried message with a valid CBC-MAC tag.

## The trick

For fixed-length CBC-MAC with block permutation $\pi$, define

$$
\operatorname{CBC}^{\ell}_{\pi}(m_1\|\cdots\|m_\ell)
=\pi(\pi(\cdots\pi(\pi(m_1)\oplus m_2)\cdots)\oplus m_\ell).
$$

Pick two first blocks $\alpha_0\neq\alpha_1$. For $j=1,\ldots,\ell-1$, query the function

$$
g_j(b\|x)=\operatorname{CBC}^{\ell}_{\pi}
(\alpha_b\|0^{(j-1)n}\|x\|0^{(\ell-j-1)n}).
$$

Because the zero blocks advance the CBC state by iterating $\pi$, this is

$$
g_j(b\|x)=\pi^{\ell-j}(\pi^j(\alpha_b)\oplus x).
$$

Since $\pi$ is injective,

$$
g_j(u)=g_j(v)
\iff
v=u\oplus(1\|z_j),
\qquad
z_j=\pi^j(\alpha_0)\oplus\pi^j(\alpha_1).
$$

So each $g_j$ is an exact Simon instance. After learning all $z_j$, output the unqueried message

$$
m=\alpha_1\|z_1\|z_2\|\cdots\|z_{\ell-1}.
$$

Classically query

$$
t_0=\operatorname{CBC}^{\ell}_{\pi}(\alpha_0\|0^{(\ell-1)n}),
\qquad
 t_1=\operatorname{CBC}^{\ell}_{\pi}(\alpha_1\|0^{(\ell-1)n}).
$$

The valid tag is $t_0$ when $\ell$ is even and $t_1$ when $\ell$ is odd. Each $z_j$ switches the CBC chain from the current branch to the other branch:

$$
\pi^j(\alpha_b)\oplus z_j=\pi^j(\alpha_{1-b}).
$$

## When to reach for it

- A chained construction exposes functions of the form $\pi^r(A_b\oplus x)$ for two branches $b\in\{0,1\}$.
- You can query superpositions over structured messages that avoid the final forged message.
- The target construction uses XOR feed-forward, so learned branch differences can be stitched into a valid chain.

## Complexity

For fixed $\ell\geq 3$, the attack uses $\ell-1$ Simon runs, hence $O(\ell n)$ coherent CBC-MAC queries, plus two classical CBC-MAC queries and polynomial linear algebra. The one-block-prefix case extends to prefixes of length $k\leq \ell-2$ by treating the prefix as an effective initial CBC state.

## Caveat

The attack needs coherent MAC queries over message superpositions. It is not an attack on a normal classical signing endpoint. Also, the proof uses fixed message length; the length-prepending variant is still affected because the attack keeps all messages at the same length.

## Related notes

- [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
- [[Weak-Period Simon Sampling for Distinguishers]]
