# Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)

> **🚧 STUB** — referenced in Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) notes.

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Peter Høyer, Michele Mosca, Ronald de Wolf |
| **Year** | 2003 |
| **Venue** | *STOC 2003* |
| **arXiv** | [arXiv:quant-ph/0209054](https://arxiv.org/abs/quant-ph/0209054) |
| **Tags** | #amplitude-amplification #recursion #quantum-walk #search |

---

## Key result

Removes the $O(\log(1/\varepsilon))$ overhead in amplitude amplification that arises when the reflection about the "good" subspace is only approximate. By applying amplitude amplification *recursively* — using a coarser amplification in the inner loop to produce a better reflection for the outer loop — the log factor is absorbed into the recursion depth with controlled error accumulation.

Achieves exact (up to $\varepsilon$) amplitude amplification in $O(1/a)$ queries (no log), where $a$ is the initial amplitude of the good state.

---

## Role in the walk-based search framework

Used in [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez-Nayak-Roland-Santha 2007]] to achieve the $O(1/\sqrt{\delta})$ quantum walk search cost without a log factor, where $\delta$ is the spectral gap of the walk. The approximate reflection comes from phase estimation on the walk operator.

---

## TODO

- [ ] Full recursion construction and error analysis
- [ ] Relation to [[Amplitude Amplification and Estimation]]
- [ ] Comparison with oblivious amplitude amplification (Berry et al.)
