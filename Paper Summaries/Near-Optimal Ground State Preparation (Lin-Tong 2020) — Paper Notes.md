> **Source:** Lin Lin and Yu Tong, *Near-optimal ground state preparation*, arXiv:2002.12508, Quantum **4**, 372 (2020)  
> **Links:** [arXiv](https://arxiv.org/abs/2002.12508) · [Quantum](https://doi.org/10.22331/q-2020-12-14-372)  
> **Tags:** #ground-state-preparation #QSP #QSVT #eigenstate-filtering #complexity

---

## What the paper does

Prepares the ground state of a Hamiltonian and estimates its ground energy, given:
- An efficiently preparable initial state |φ₀⟩ with overlap ⟨ψ₀|φ₀⟩ ≥ γ (ψ₀ = ground state)
- A lower bound on the spectral gap Δ = λ₁ − λ₀
- Optionally: an upper bound μ on the ground energy

Achieves logarithmic dependence on 1/ε (inverse error), exponentially better than phase estimation approaches.

## Main idea

1. **Block-encode the Hamiltonian** H as an (α, m, 0)-block-encoding U_H.
2. **Use QSP/QSVT** to implement an approximate sign polynomial on the eigenvalues.
3. **Build a reflector** R<μ ≈ −sign(H − μI), then a projector P<μ = (I + R<μ)/2 via simple Hadamard gadget.
4. **Apply the projector** to |φ₀⟩ to extract the ground state component.
5. **Amplitude amplify** to boost success probability (adds 1/γ factor).

When μ is unknown: binary search using the projector as an oracle test, combined with binary amplitude estimation.

## Two algorithms

### Case 1: Ground energy upper bound μ known

| Oracle | Complexity |
|---|---|
| U_H queries | O( (α/(γΔ)) · log(1/ε) ) |
| U_I (initial state) queries | O(1/γ) |

Logarithmic in $1/\epsilon$ — the key improvement over [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev phase estimation]] (which gives polynomial dependence).

### Case 2: Ground energy unknown — hybrid algorithm

Uses binary search over energy values. Each test uses the projector-block-encoding plus amplitude estimation to decide which side of λ₀ a candidate lies.

| Task | Complexity |
|---|---|
| Energy estimation to precision h | Ũ_H = O(α/(γh)), Ũ_I = O( (1/γ) · polylog ) |
| Full state preparation (combine) | Ũ_H = O(α/(γΔ) · log(1/ε)), Ũ_I = O( (1/γ) · polylog ) |

Note: U_I dependence goes from O(1/√h) in Ge et al. (2019) to polylogarithmic here — exponential improvement.

## Comparison to Ge et al. (2019)

| Metric | This work | Ge et al. |
|---|---|---|
| U_H (prep, μ known) | O(α/(γΔ) · log(1/ε)) | Ũ(α/(γΔ)) |
| U_H (energy est. to h) | Ũ(α/(γh)) | Ũ(α^{3/2}/(γh^{3/2})) |
| U_H (prep, μ unknown) | Ũ(α/(γΔ) · log(1/ε)) | Ũ(α^{3/2}/(γΔ^{3/2})) |
| U_I dependence | polylog in precision | algebraic |

Key improvements: better dependence on h (h^{−3/2} → h^{−1}) and on Δ (Δ^{−3/2} → Δ^{−1}); exponential improvement in U_I calls with respect to precision.

## Why QSP matters here

The critical technical piece is the polynomial filter. Using QSP:
- Degree = O((α/Δ) · log(1/ε)) — gives the log(1/ε) scaling
- Previous approaches using LCU or phase estimation give polynomial dependence on 1/ε

The approximate sign polynomial construction (Lemma 3) is the workhorse.

## Lower bounds

The paper proves near-optimality via reductions:
- Unstructured search → Ω(1/γ) and Ω(1/Δ) queries necessary
- Quantum approximate counting → Ω(1/h) for energy estimation

These match (up to log factors) the achieved scalings.

## Caveats

- Requires a lower bound on the spectral gap Δ
- Initial state overlap γ matters: 1/γ appears in query complexity
- If no upper bound on ground energy, need the hybrid algorithm (more complex, more queries)
- For very small γ, the 1/γ factor can be prohibitive in practice

## Related notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSP/QSVT framework
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP for Hamiltonian simulation
- [[Linear Combination of Unitaries (LCU)]] — alternative approach
- [[Chebyshev Polynomial Spectral Projector]] — related filtering technique
- [[Gapped Phase Estimation]] — alternative gap estimation
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]

## References within this paper

- Ge et al. (2019) — prior work on ground state preparation (J. Math. Phys. 60, 022202)
- Gilyén et al. (2019) — QSVT framework
- Low & Chuang (2017) — optimal Hamiltonian simulation via QSP
- Poulin & Wocjan (2009) — earlier ground state preparation approach
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin and Tong (2022)]] — same authors' follow-up: Heisenberg-limited energy estimation with 1 ancilla, designed for early fault-tolerant hardware