# Hamming-Distance Ordering for Multi-Determinant Preparation

> **Source:** Tubman, Mejuto-Zaera, Epstein, Hait, Levine, Huggins, Jiang, McClean, Babbush, Head-Gordon, Whaley, arXiv:1809.05523 (2018)
> **Tags:** #trick #state-preparation #multi-determinant #circuit-optimisation #classical-preprocessing

## What it does

Reduces the total gate count in [[Sequential Multi-Determinant State Preparation]] by ordering the determinants $\{D_\ell\}$ so that Hamming distances between successive pairs $\text{d}_H(D_\ell, D_{\ell+1})$ are small. Directly reduces the number of qubit flips needed per inductive step.

## The trick

In the sequential state preparation protocol, each step $\ell \to \ell+1$ requires applying X gates on every qubit position where $D_\ell$ and $D_{\ell+1}$ differ (after the rotation and erase sub-steps). The cost per step scales as $O(n)$ in the worst case, but the constant depends on the Hamming distance $\text{d}_H(D_\ell, D_{\ell+1})$ between consecutive determinants.

Specifically, the number of X gates per inductive step is at most $\text{d}_H(D_\ell, D_{\ell+1}) - 1$ (one differing bit is handled by the rotation in step 1, the remaining $\text{d}_H - 1$ by explicit X flips).

**Total cost with ordering:**

$$\text{Gate count} \;\propto\; \sum_{\ell=1}^{L-1} \text{d}_H(D_\ell, D_{\ell+1})$$

**Minimising this sum** is equivalent to finding a minimum-weight Hamiltonian path through $L$ points in $\{0,1\}^n$ with the Hamming distance as the edge weight. This is NP-hard in general, but:
- For $L \ll 2^n$ (which is the regime of interest), greedy nearest-neighbour ordering works well.
- Physical structure helps: CI wavefunctions have top determinants that typically differ by single or double excitations from HF ($\text{d}_H = 2$ or $4$), so the average Hamming distance between important determinants is small.

**Greedy algorithm:**
1. Start from $D_1 = D_\text{HF}$ (or the dominant determinant from ASCI).
2. At each step, choose the unvisited determinant with smallest Hamming distance to the current one.
3. Result: ordering that favours local moves in occupation-number space.

In second-quantized CI wavefunctions, most pairs of top-contributing determinants differ by exactly 2 or 4 orbitals (single or double excitations), so the total gate overhead is typically $O(L)$ extra gates above the $O(nL)$ baseline, not $O(nL)$ extra.

## When to reach for it

- Whenever using [[Sequential Multi-Determinant State Preparation]] with $L > 10$ determinants.
- As a classically cheap preprocessing step (greedy $O(L^2)$ algorithm) that can reduce quantum gate counts by a constant factor — worth doing before any circuit synthesis.
- When targeting fault-tolerant implementations where each gate has a cost: reducing the constant in $O(nL)$ by even $2\times$ matters.

## Complexity

**Classical preprocessing:** $O(L^2 \cdot n)$ for brute-force nearest-neighbour; $O(L^2)$ if Hamming distances are precomputed. For $L \leq 10^3$, this is negligible.

**Gate count reduction:** Depends on the dataset. For CI wavefunctions (top determinants = excitations from HF), expect $\text{d}_H \approx 2$–$4$ per step on average, vs. $O(n)$ in worst-case random ordering. Effective gate count reduction: $O(n/4)$ factor in typical cases.

## Caveat

- **Greedy ordering is not optimal.** The minimum Hamiltonian-path problem is NP-hard. In practice, greedy works well because the physical structure of CI wavefunctions keeps Hamming distances between important determinants small.
- **No rigorous bound stated in the paper.** The paper mentions Hamming ordering as a cost reduction technique but does not prove a tight bound on how much it helps for typical quantum chemistry instances.
- **Not directly applicable to Method A.** This ordering trick is specific to the sequential auxiliary-qubit construction (Method B). Method A (compressed register + QROM) does not benefit from this ordering.

## Related notes
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — introduced here
- [[Sequential Multi-Determinant State Preparation]] — the protocol this optimises
- [[ASCI Overlap Estimation for QPE Initial State]] — ASCI provides the set of determinants $\{D_\ell\}$ to be ordered
