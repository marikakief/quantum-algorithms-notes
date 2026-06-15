# Review: Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016)

Source note: [[Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) — Paper Notes]]

## Primary Sources Checked

- Roetteler, "Quantum algorithms for abelian difference sets and applications to dihedral hidden subgroups", arXiv:1608.02005 (2016).

## Verdict

Minor issues. The note is accurate and appropriately limits the result to structured, known-template hidden shifts. The main caveat is that "dihedral HSP instances" here are white-box/tiny-slice instances, not worst-case DHSP.

## Line-Anchored Comments

- Lines 18-24: good hidden-shift problem statement. Mention explicitly that the unshifted difference set is known.
- Lines 26-30: the main conceptual move is well stated.
- Lines 34-47: the spectral-correction algorithm is clear. The diagonal correction must be efficiently computable for the claimed algorithmic efficiency.
- Lines 50-52: good family examples. Paley/Hadamard/Singer should be tied to the corresponding efficient implementation assumptions.
- Lines 56-60: key results are accurate. The classical-hardness statement should be stated for the specific white-box Singer family, not generic dihedral HSP.
- Lines 62-66: assessment is well scoped.

## Missing or Suggested Additions

- Add one sentence distinguishing injective hidden shift from shifted characteristic functions of difference sets; the latter are non-injective but spectrally structured.

