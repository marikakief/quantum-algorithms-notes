# Coset Representation for Approximate Modular Arithmetic

> **Source:** Christof Zalka, quant-ph/0601097 (2006); formalised by Craig Gidney, arXiv:1905.08488 (2019)
> **Tags:** #trick #quantum-arithmetic #approximate #modular-arithmetic #Shor

## What it does

Encodes a modular integer $r \bmod N$ as a superposition so that modular addition reduces to ordinary (non-modular) addition, eliminating the comparisons and conditional subtractions that make exact modular adders $\sim 5\times$ more expensive than non-modular adders.

## The trick

Represent $r \bmod N$ with $m$ padding qubits as:

$$|\text{Coset}_m(r)\rangle = \frac{1}{\sqrt{2^m}} \sum_{j=0}^{2^m - 1} |r + jN\rangle$$

This state is approximately a $+1$ eigenvector of the "add $N$" operation: adding $N$ shifts the superposition by one period, but the boundary term (where $j = 2^m - 1$ wraps around) has weight $1/2^m$. So adding any multiple of $N$ barely changes the state.

To add $k \bmod N$: just add $k$ (non-modularly) into the encoded register. No comparison against $N$, no conditional subtraction. The non-modular sum $r + k$ might exceed $N$, but in the coset representation the excess is absorbed into the superposition over multiples of $N$.

**Deviation:** $\leq 2^{-m}$. Only the single coset value $c = 2^m - 1$ (at the register boundary) can produce a wrong answer. Via the [[Approximate Encoded Permutation Framework]], this means trace distance $\leq 2\sqrt{2^{-m}}$ per addition.

**Encoding circuit:** Prepare $m$ ancilla qubits in $|+\rangle$. For each ancilla $j$: conditionally add $2^j N$ to the data register, negate amplitudes where the register $\geq 2^j N$ (using a comparison circuit toggling a $|-\rangle$ qubit), then Hadamard and measure the ancilla. Cost: $O(nm)$ Toffolis where $n = \lceil \log_2 N \rceil$.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: modular exponentiation requires $O(n^2)$ modular additions. Each exact modular addition costs $\sim 5$ non-modular additions (add, compare, conditionally subtract, compare, fix). With the coset representation, each modular addition is one non-modular addition — a $5\times$ reduction in gate count and depth.
- Any algorithm performing many modular additions into the same register.
- Combines naturally with [[Oblivious Carry Runway|oblivious carry runways]]: the coset encoding is the outermost layer (modular $\to$ non-modular), and carry runways are the inner layer (long addition $\to$ piecewise addition).

## Complexity

| Resource | Cost |
|---|---|
| Encoding overhead | $O(nm)$ Toffolis (one-time) |
| Extra qubits | $m$ (padding) |
| Per-addition savings | $\sim 5\times$ reduction vs. exact modular addition |
| Deviation per addition | $\leq 2^{-m}$ |

## Caveat

- The encoding circuit itself costs $O(nm)$ Toffoli gates. This is amortised over $k$ additions, so it's only worthwhile when $k$ is large (as in Shor's algorithm with $k = O(n^2)$).
- The total deviation after $k$ additions is $\leq k \cdot 2^{-m}$ (by subadditivity). For Shor with $k = O(n^2)$, padding $m = 2\log_2 n + O(1)$ suffices for inverse-polynomial total error. This is a modest qubit overhead.
- The idea is due to Zalka (2006); Gidney (2019) contributed the formal error analysis via the [[Approximate Encoded Permutation Framework]].

## Related notes
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — formalises the error analysis
- [[Approximate Encoded Permutation Framework]] — the composable error framework
- [[Oblivious Carry Runway]] — the complementary technique for depth reduction; typically used inside the coset encoding
- [[Arithmetic in the Fourier Basis]] — a different approach to avoiding carry overhead: work in the QFT basis instead of using coset superpositions
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the primary application
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — an exact modular multiplication approach that does not use the coset representation
