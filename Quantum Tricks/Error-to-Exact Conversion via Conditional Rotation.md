# Error-to-Exact Conversion via Conditional Rotation

> **Source:** Ettinger, Høyer, Knill, arXiv:quant-ph/0401083 (2004)
> **Tags:** #trick #amplitude-amplification #exact-algorithm #error-reduction

## What it does

Converts a bounded-error quantum algorithm (whose success probability depends on the hidden answer) into an exact algorithm (zero error), by using precomputed probability information to equalize success probabilities across answers, then applying [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]].

## The trick

Suppose a quantum algorithm produces measurement outcomes with answer-dependent probabilities, captured in a matrix $M$ where $M_{H,\mu} = \Pr[\text{output } \mu \mid \text{answer is } H]$.

1. **Compute $M$ classically:** Simulate the quantum algorithm on each possible answer to determine all entries of $M$. (This may take exponential time — that's acceptable for query complexity results.)

2. **Design equalizing rotations:** Choose a target probability vector $y$ (e.g., $y_H = 3/4$ if $H \in Y_{3/4}$, $y_H = 1/4$ otherwise). Solve $x = M^{-1}y$. When $M \approx I$ (high-probability algorithm), $M^{-1}$ exists and $x$ has entries in $[0,1]$.

3. **Conditional rotation:** After the main algorithm outputs $\mu$, rotate an ancilla qubit to $\sqrt{1-x_\mu}|0\rangle + \sqrt{x_\mu}|1\rangle$. The probability of measuring $|1\rangle$ is $\sum_\mu x_\mu M_{H,\mu} = (Mx)_H = y_H$ — exactly the target probability, independent of the specific answer $H$.

4. **Amplify to exact:** [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Amplitude amplification]] converts $3/4$ vs $1/4$ to $1$ vs $0$, giving exact discrimination between $Y_{3/4}$ and $Y_{1/4}$.

5. **Binary search:** Repeat with different partitions $(Y_{3/4}, Y_{1/4})$ to identify the answer exactly. For $r$ possible answers, $O(\log r)$ rounds suffice.

## When to reach for it

- Converting bounded-error quantum algorithms to exact ones when you can precompute the probability matrix
- Proving query complexity upper bounds for exact computation (the exponential-time preprocessing is irrelevant to query counting)
- Any setting where the number of possible answers is manageable and you need zero-error guarantees

## Complexity

Adds $O(\log r)$ rounds of amplification on top of the base algorithm, where $r$ is the number of possible answers. Each round uses a constant number of base algorithm calls. The classical preprocessing for $M^{-1}$ may be exponential.

## Caveat

Requires precomputing the full probability matrix $M$ — feasible only when the algorithm can be simulated classically for each possible answer. This makes the technique useful for proving query complexity bounds but not for building efficient algorithms. Also requires arbitrary-angle single-qubit rotations; a restricted gate set may not suffice for the exact version.

## Related notes

- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Sequential Periodicity Testing via Projective Filtering]]
- [[Standard Amplitude Amplification]]
