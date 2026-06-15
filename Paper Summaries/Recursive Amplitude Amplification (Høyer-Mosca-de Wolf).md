> **🚧 STUB** — corrective note for Høyer-Mosca-de Wolf's bounded-error-input search theorem, referenced by walk-search notes.

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Peter Høyer, Michele Mosca, Ronald de Wolf |
| **Year** | 2003 |
| **Title** | *Quantum Search on Bounded-Error Inputs* |
| **Venue** | *ICALP 2003*, LNCS 2719, pp. 291-299 |
| **arXiv** | [arXiv:quant-ph/0304052](https://arxiv.org/abs/quant-ph/0304052) |
| **Tags** | #amplitude-amplification #recursion #quantum-walk #search |

---

## Key result

Problem: there are $n$ base algorithms, each computing a bit with bounded error probability. We want to find an index whose bit is 1, if such an index exists.

Naively, one could first reduce each base algorithm's error to $O(1/\operatorname{poly}(n))$ and then run Grover search, costing an extra $O(\log n)$ factor. Høyer-Mosca-de Wolf avoid this: by recursively interleaving amplitude amplification with error reduction, they find a 1-index with high probability using only $O(\sqrt{n})$ repetitions of the base algorithms.

This is not a generic "exact amplitude amplification in $O(1/a)$" theorem. Its point is that amplitude amplification can still work when the predicate/verifier has only bounded error, without first making every predicate evaluation highly reliable.

---

## Role in the walk-based search framework

This result is conceptually relevant to later walk-search frameworks because quantum walks often supply approximate or bounded-error reflections/verifiers. The Høyer-Mosca-de Wolf theorem shows how to avoid paying a blanket logarithmic reliability overhead before search. In [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS-style walk search]], this supports the idea that approximate checking/reflection procedures must be handled inside the amplification analysis, not simply union-bounded as independent high-confidence subroutines.

---

## TODO

- [ ] Full recursion construction and error analysis
- [ ] Relation to [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [ ] Comparison with oblivious amplitude amplification (Berry et al.)
