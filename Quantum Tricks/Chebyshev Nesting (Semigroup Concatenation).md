# Chebyshev Nesting (Semigroup Concatenation)

> **Source:** Yoder, Low, Chuang, arXiv:1409.3305
> **Tags:** #trick #amplitude-amplification #fixed-point #Chebyshev #adaptive #concatenation

## What it does

Concatenates two fixed-point amplitude amplification sequences of complexities $L_1$ and $L_2$ into one of complexity $L_1 L_2$, without restarting from the initial state.

## The trick

The Chebyshev polynomials satisfy the **semigroup property**: $T_p(T_q(x)) = T_{pq}(x)$.

After running a fixed-point sequence $S_{L_1}$ with error parameter $\delta_1$, the output state has success probability $P_{L_1}(\lambda) = 1 - \delta_1^2 [T_{L_1}(\ldots)]^2$. Treat this output as the "initial state" for a second sequence $S_{L_2}$ with error $\delta_2 = \delta$. The composition gives:

$$P_{L_2}(P_{L_1}(\lambda, \delta_1), \delta_2) = P_{L_1 L_2}(\lambda, \delta)$$

provided we choose $\delta_1 = T_{1/L_2}(1/\delta)^{-1}$.

**Operationally:** replace the state preparation $A$ inside the second sequence $S_{L_2}$ with $S_{L_1} \cdot A$.

The phase angles for the nested sequence are:

$$\alpha_j^{(1,2)} = \begin{cases} \alpha_h^{(1)} & j \equiv h \pmod{L_1} \\ -\alpha_h^{(1)} & j \equiv -h \pmod{L_1} \\ \alpha_k^{(2)} & j = kL_1 \end{cases}$$

with phase matching $\beta_j = -\alpha_{l-j+1}$.

## When to reach for it

- **Adaptive search:** run $S_{L_1}$, check if the result is good enough, extend to $S_{L_1 L_2}$ if not — no restart needed, because $S_{L_1}$ is a prefix of the nested sequence.
- **Multiplicative query complexity composition:** combine a cheap coarse search with a finer refinement.
- **Composite pulse design:** the same concatenation structure appears in NMR composite pulse sequences (Jones 2013).
- **Anywhere Grover is used iteratively** — nesting avoids the bookkeeping of "how many iterations should I run."

## Complexity

A nested sequence of complexities $L_1, L_2$ uses $l_1 + 2l_1 l_2 + l_2$ Grover iterates (where $L_i = 2l_i + 1$). For $k$-level nesting: $L_1 L_2 \cdots L_k$ total complexity, but the number of physical iterates grows multiplicatively.

## Caveat

- Each nesting level multiplies the circuit depth. For many levels, the depth can become impractical.
- The error parameter $\delta_1$ for the inner sequence depends on the outer sequence's target — must be planned in advance (or computed adaptively).
- Nesting preserves the Chebyshev structure specifically because the phases from the Yoder-Low-Chuang construction (Eq. 11) have the right palindromic symmetry. Other phase choices won't nest as cleanly.

## Related notes
- [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]]
- [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]]
- [[Standard Amplitude Amplification]]
- [[Recursive Amplitude Amplification with Approximate Reflections]]
