# Review: Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024)

Source note: [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2410.21258

## Verdict

Minor issues. This is a strong and careful summary of a recent paper. The main caution is to keep the precise promise problem visible: weighted clique complexes, subset-state input, spectral-gap promise, and BQP_1 rather than full BQP hardness.

## Comments

- "Strongest result in the TDA advantage story" is a defensible assessment, but it is time-sensitive. Date it or phrase it as "among the strongest current results" if the original note is edited.

- BQP_1-hardness should not be casually collapsed to BQP-hardness. The note correctly says equality with BQP is open; keep that caveat attached to the advantage claim.

- The weighted-complex requirement is central. It belongs in the computational-problem statement and theorem summary, not only in the limitations.

- The input state is a succinct subset-state/harmonic representative, not an arbitrary TDA feature supplied by a classical persistent-homology pipeline.

- Harmonic persistence is related to, but not identical with, standard barcode persistence. The false-positive/false-negative discussion is excellent and should remain prominent.

## Suggested Additions

- Add a "promise problem" box listing input, YES/NO promises, gap promise, and output.
- Add a one-line explanation that containment in BQP uses QPE on the Laplacian and repeated detection of zero eigenvalue overlap.
