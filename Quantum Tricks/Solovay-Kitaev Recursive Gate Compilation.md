# Solovay-Kitaev Recursive Gate Compilation

> **Source:** Solovay (1995), Kitaev (1997); pedagogical presentation in Dawson & Nielsen, arXiv:quant-ph/0505030
> **Tags:** #trick #gate-compilation #universality #fault-tolerance #fundamental

## What it does

Compiles any single-qubit gate $U$ into a sequence of $O(\log^{3.97}(1/\epsilon))$ gates from a fixed universal gate set $G$, with accuracy $\epsilon$ in operator norm. The classical algorithm that finds the sequence runs in $O(\log^{2.71}(1/\epsilon))$ time.

## The trick

Recursive improvement via [[Group Commutator Error Cancellation|group commutator error cancellation]]:

1. Start with a coarse $\epsilon_0$-approximation $U_0$ (lookup table)
2. Compute the error $\Delta = U U_{n-1}^\dagger$ (close to identity)
3. Decompose $\Delta = VWV^\dagger W^\dagger$ via [[Balanced Group Commutator Decomposition]] ($V, W$ are $O(\sqrt{\epsilon_{n-1}})$-close to identity)
4. Recursively approximate $V$ and $W$ to accuracy $\epsilon_{n-1}$
5. The commutator of the approximations has accuracy $\epsilon_n = O(\epsilon_{n-1}^{3/2})$

Recurrence: $\epsilon_n = c \cdot \epsilon_{n-1}^{3/2}$ → doubly exponential convergence.

## When to reach for it

- Compiling continuous rotations (e.g., Shor's $\pi/2^k$ gates) into a discrete fault-tolerant gate set (Clifford + T)
- Any situation where a circuit uses gates not in the available gate set
- Understanding why "universal gate set" is a practical rather than just theoretical concept

## Complexity

- Sequence length: $O(\log^{3.97}(1/\epsilon))$
- Algorithm runtime: $O(\log^{2.71}(1/\epsilon))$ (using pointer-based output)
- Not optimal: Harrow-Recht-Chuang showed $O(\log(1/\epsilon))$ is achievable for some gate sets. Modern methods (Ross-Selinger 2014) achieve this constructively for Clifford + T.

## Caveat

The $\log^{3.97}$ exponent is loose. In practice, the constant matters more than the exponent for reasonable $\epsilon$. The lookup table precomputation for the base case can be expensive for $SU(d)$ with large $d$ (scales as $O(1/\epsilon_0^{d^2-1})$).

## Related notes

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
- [[Group Commutator Error Cancellation]]
- [[Balanced Group Commutator Decomposition]]
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]]
