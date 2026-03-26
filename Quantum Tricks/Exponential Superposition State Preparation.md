# Exponential Superposition State Preparation

> **Source:** Berry, Cleve, Gharibian, arXiv:1211.4637
> **Tags:** #trick #state-preparation #amplitude-encoding

## What it does

Prepares the state $|\varphi_q\rangle = \sum_{s=0}^{q-1}\beta\alpha^s|s\rangle + \alpha^q|q\rangle$ using $O(\log q)$ gates, where $\alpha^2 + \beta^2 = 1$. This is a geometrically-weighted superposition over a register of dimension $q+1$.

## The trick

For $q = 2^r$, define the single-qubit rotation:

$$M(\gamma) = \frac{1}{\sqrt{1+\gamma^2}}\begin{pmatrix} 1 & -\gamma \\ \gamma & 1 \end{pmatrix}$$

Then:

$$M(\alpha^{2^{r-1}}) \otimes \cdots \otimes M(\alpha^2) \otimes M(\alpha)|0^r\rangle = \frac{1}{\sqrt{1-\alpha^{2q}}}\sum_{s=0}^{q-1}\beta\alpha^s|s\rangle$$

This works because the $j$-th rotation mixes states differing in bit $j$, creating a product of conditional rotations that yields geometric weights in the binary representation.

To get the full state $|\varphi_q\rangle$ including the $\alpha^q|q\rangle$ tail, add one extra qubit initialised to $\sqrt{1-\alpha^{2q}}|0\rangle + \alpha^q|1\rangle$ and condition the rotations on this qubit being $|0\rangle$. Total cost: $r+1 = O(\log q)$ gates.

## When to reach for it

- You need a superposition with geometrically decaying amplitudes
- Direct preparation would touch all $q$ basis states ($O(q)$ gates), but you only need $O(\log q)$
- Building blocks for compressed encodings where the "natural" state is a product of these exponential states

The technique generalises: any product-form amplitude structure over binary-indexed states admits similar logarithmic-depth preparation.

## Complexity

- $O(\log q)$ single-qubit rotations (each a different $M(\alpha^{2^j})$)
- 1 additional qubit for the tail state
- Total: $r+2$ qubits and $r+1$ gates for $q = 2^r$

## Caveat

The rotations $M(\alpha^{2^j})$ involve angles that depend on the parameter $\alpha$, which itself depends on $m$ (the number of logical qubits being compressed). For fault-tolerant implementations, these rotations need to be synthesised to precision $O(\varepsilon/k)$ using $O(\log(k/\varepsilon))$ $T$-gates each.

## Related notes

- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] — source paper
- [[Succinct Encoding of Low-Hamming-Weight Superpositions]] — the encoding this state prepares
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]] — an alternative state preparation approach mentioned as a possible substitute
