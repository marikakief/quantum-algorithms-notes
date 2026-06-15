# Review: Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016)

Source note: [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]

## Primary Sources Checked

- O'Malley et al., "Scalable Quantum Simulation of Molecular Energies," PRX 6, 031007 (2016) / arXiv:1512.06860.

## Verdict

Minor issues. The note is detailed and correctly emphasizes that "scalable" refers to the fermion-to-qubit representation and compilation model, not to the two-qubit H2 experiment itself.

## Line-Anchored Comments

- Lines 21-23: good distinction between VQE and PEA performance. Preserve the point that VQE's feedback loop absorbs coherent hardware errors in this small instance.
- Lines 30-32: the Bravyi-Kitaev and symmetry-reduction story is correct at a high level. State explicitly which two qubits are tapered/frozen by symmetries rather than by the Hartree-Fock state alone.
- Lines 42-50: "11-gate circuit" conflicts with "11 single-qubit + 2 CZ" later in the same paragraph. Separate single-qubit gates from entangling gates.
- Lines 75-85: the PEA failure reason is right: one Trotter step was hardware-limited. Avoid implying PEA is intrinsically less accurate; fault-tolerant PEA is the high-precision algorithm.
- Lines 102-111: the comparison with earlier non-scalable configuration-basis demonstrations is one of the note's strongest parts.
- Lines 143-157: many references are marked "No" despite existing vault notes or related trick cards. Before publishing, either update the vault-note column or remove the column.

## Missing or Suggested Additions

- Add a compact sentence defining "scalable" in this paper: polynomially generated qubit Hamiltonian and circuits via a fermionic encoding, even though the demonstrated molecule is still classically trivial.
