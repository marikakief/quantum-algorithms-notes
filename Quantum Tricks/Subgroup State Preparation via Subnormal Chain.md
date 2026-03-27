
> **Source:** Watrous, arXiv:quant-ph/0011023 (2001), §3.2
> **Tags:** #trick #group-theory #solvable-groups #state-preparation #non-abelian

## What it does

Converts $l$ copies of $|H\rangle = |H|^{-1/2}\sum_{h \in H}|h\rangle$ into $l - 1$ copies of $|\langle g \rangle H\rangle$ (the uniform superposition over the next larger subgroup in a subnormal chain), given that $g$ normalises $H$ and the relative order $r = r_H(g)$ is known.

## The trick

For each copy $R_i$ with ancilla $A_i \in \mathbb{Z}_r$:
1. QFT$_r$ on $A_i$, left-multiply $R_i$ by $g^{a_i}$, QFT$_r$ on $A_i$
2. Measure $A_i \to b_i$

State of $R_i$ after measurement: $|\psi_i\rangle = \frac{1}{\sqrt{r}}\sum_{a_i} e_r(a_i b_i)|g^{a_i}H\rangle$

**The eigenstate-as-phase-reference trick:** Pick any $k$ with $\gcd(b_k, r) = 1$. The state $|\psi_k\rangle$ is an **eigenvector** of the coset multiplication operator:

$$
M_{g^j h}|\psi_k\rangle = e_r(-j b_k)|\psi_k\rangle
$$

For each $i \neq k$: left-multiply $R_i$ by $R_k$ raised to power $c \equiv b_i b_k^{-1} \pmod{r}$. This cancels all phases in $|\psi_i\rangle$, producing:

$$
R_i \to \frac{1}{\sqrt{r|H|}} \sum_{a_i \in \mathbb{Z}_r} \sum_{h \in H} |g^{a_i}h\rangle = |\langle g \rangle H\rangle
$$

The state $|\psi_k\rangle$ is used up (consumed as the phase reference), giving $l - 1$ good copies from $l$ inputs.

## When to reach for it

- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous's solvable group algorithm]]: climb the subnormal chain $\{1\} = H_0 \trianglelefteq H_1 \trianglelefteq \cdots \trianglelefteq H_m = G$
- Any setting where you need to prepare uniform superpositions over progressively larger subgroups
- Quantum algorithms on groups where you can't classically enumerate group elements but can multiply them

## Complexity

Per conversion step: $O(\log(1/\epsilon))$ QFT$_r$ applications + group oracle calls. The QFT over $\mathbb{Z}_r$ is $O(\log^2 r)$ gates. One copy is consumed per step.

Total for the full chain ($m$ steps): $O(m \cdot k)$ copies needed at the start, where $k = O(n \log(m/\epsilon))$.

## Caveat

Requires $g$ to **normalise** $H$ (i.e., $gHg^{-1} = H$, so $\langle g \rangle H$ is a group). This is guaranteed by the subnormal series structure of solvable groups, but won't work for arbitrary subgroup extensions. Also requires at least one measured $b_i$ coprime to $r$ — this happens with probability $\phi(r)/r$ per trial, which is $\Omega(1/\log\log r)$.

## Related notes

- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[Quantum States as Coset Representatives]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Coset Sampling via Fourier Transform]]
