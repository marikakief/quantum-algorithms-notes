# Review: Quantum Computation and Lattice Problems (Regev 2002)

Source note: [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]

## Primary Sources Checked

- Regev, "Quantum Computation and Lattice Problems", FOCS 2002; arXiv:cs/0304005.

## Verdict

Major issues. The mathematical story is mostly right, but several phrasings overstate the result's later role and one theorem label is too compressed. This note should also add a normal title heading; it currently starts at the source block.

## Line-Anchored Comments

- Lines 1-5: missing top-level title. Add `# Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes` for consistency with the rest of the vault.
- Lines 17-24: the two-reduction chain is correct, but "later became the foundation of post-quantum cryptography" is too strong. Regev's later LWE work became foundational; this paper is an important predecessor and conceptual bridge.
- Lines 30-43: the unique-SVP to two-point construction is summarized well. The ball-versus-cube improvement is the right geometric intuition.
- Lines 45-53: the DCP-to-subset-sum reduction is clear. Say "density about one" explicitly here, because it is central to the limitations.
- Lines 59-65: the theorem summary should be less slogan-like. In particular, line 65 should read as "an efficient solution to the relevant dihedral coset problem/dihedral HSP would imply..." rather than "coset sampling implies"; coset sampling alone is information, not an efficient solver.
- Lines 73-78: the comparison to later LWE is useful, but ensure the gap statements do not imply this 2002 reduction was the deployed basis of lattice cryptography.
- Lines 84-90: the caveats are strong. The conditional nature and unique-SVP restriction should appear in the opening assessment as well.
- Lines 112-115: now that the vault has Kuperberg and Regev DHSP notes, update "no vault note" references where applicable.

## Missing or Suggested Additions

- Add a short notation note distinguishing the lattice dimension `n` from the DCP modulus `N`; the summary switches between them quickly.
- Add a line explaining that Kuperberg's subexponential DHSP algorithm gives a subexponential algorithm through this route only for the corresponding unique-SVP parameters, not for general SVP or LWE.

