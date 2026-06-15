# Review: Fermionic SWAP for Anticommutation Enforcement

Source note: [[Fermionic SWAP for Anticommutation Enforcement]]

## Primary Sources Checked

- Verstraete, Cirac, and Latorre, *Quantum circuits for strongly correlated quantum systems*, arXiv:0804.1888: https://arxiv.org/abs/0804.1888
- Kivlichan et al., *Quantum Simulation of Electronic Structure with Linear Depth and Connectivity*, arXiv:1711.04789: https://arxiv.org/abs/1711.04789

## Verdict

Minor issues. The matrix and conceptual role are correct. The card should avoid implying that fSWAP is the only way to handle nonadjacent fermionic interactions; parity strings and alternative encodings are also valid.

## Line-Anchored Comments

- Lines 11-15: The fSWAP matrix is correct in the occupation basis, with the `-1` on `|11>`.

- Line 15: "SWAP followed by CZ" is fine because CZ is symmetric with respect to the two qubits. If decomposing into CNOTs, specify that the exact count depends on the native gate set.

- Line 17: "Must use fermionic SWAPs whenever non-adjacent modes need to interact" is too strong. A JW circuit may instead implement a long parity string directly. fSWAP is required when the compilation strategy physically reorders fermionic modes.

- Lines 21-24: Good use cases: FFFT, swap networks, Bogoliubov/Givens transformations.

- Lines 28-32: The overhead comparison is fair for routing-based approaches. It should say the dominant cost is architecture- and compilation-dependent.

## Missing or Suggested Additions

- Add the key identity: fSWAP swaps mode labels, not just qubit states.
- Mention that tracking mode order classically after fSWAPs is part of the compilation model.
