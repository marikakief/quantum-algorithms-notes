# Singer Difference Sets as Dihedral HSP Instances

## Pattern

Singer difference sets in cyclic groups provide hidden-shift instances that can be embedded into dihedral HSP. For $N=2^n-1$, the resulting white-box dihedral-HSP instances have efficient quantum algorithms and no heavy classical post-processing.

## Construction idea

1. Use a Singer difference set $D$ in a cyclic group of order $N$.
2. Hide a shift of its characteristic function.
3. Use the hidden-shift to generalized-dihedral-HSP equivalence.
4. Apply the difference-set correlation algorithm to recover the shift.

## Caveat

The family is small compared with all possible dihedral-HSP hiding functions. It gives explicit easy instances, not an efficient algorithm for adversarial dihedral HSP.

## Source paper

- [[Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) — Paper Notes|Roetteler (2016)]].
