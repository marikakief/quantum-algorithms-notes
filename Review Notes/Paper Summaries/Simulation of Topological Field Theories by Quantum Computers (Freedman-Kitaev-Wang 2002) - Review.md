# Review: Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002)

Source note: [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/quant-ph/0001071
- CMP DOI linked in the source note.

## Verdict

Minor issues. This is a strong conceptual summary. The main edits should narrow the Church-thesis language and keep the distinction between unitary mapping-class-group simulation and partial/probabilistic bordism simulation clear.

## Line-Anchored Comments

- Lines 9-12: Good statement of why ordinary Hamiltonian simulation is not the right lens: TQFT state spaces and mapping-class-group actions are the objects to simulate.

- Lines 17-25: The conclusion "TQFTs don't give extra computational power" should be scoped to the paper's algebraic unitary topological modular functor setting and the specified mapping-class/bordism operations. Avoid presenting it as a theorem about every object called a TQFT.

- Lines 31-43: The axioms table is helpful. The algebraicity condition is especially important and should be mentioned in the theorem statement as well.

- Lines 47-60: The pants-decomposition embedding is clearly explained. Add that the tensor-product embedding depends on a chosen decomposition; the algorithm changes decompositions to make generators local.

- Lines 83-86: The bordism extension is not ordinary unitary circuit simulation. It uses measurements/postselection-like partial circuits that can abort. This should be separated sharply from Theorem 2.2's exact unitary simulation of diffeomorphisms.

- Lines 91-95: Good theorem bullets. Keep `c(V)` dependence visible: the finite gate set/local dimension constants depend on the chosen modular functor.

- Lines 113-116: The noisy/fault-tolerant comment is plausible context, but not a result of this paper's simulation theorem. Label it as later fault-tolerance context.

## Missing or Suggested Additions

- Add a note distinguishing this paper from Freedman-Larsen-Wang universality: this paper is "TQFT in BQP"; the companion direction is "some TQFTs can realize BQP."

- Add a small glossary for pants decomposition, F-move, S-move, Dehn twist, and mapping class group. The note uses them well but densely.

