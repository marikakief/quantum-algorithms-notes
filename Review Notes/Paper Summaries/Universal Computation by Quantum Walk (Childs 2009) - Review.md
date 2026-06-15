# Review: Universal Computation by Quantum Walk (Childs 2009)

Source note: [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]]

## Primary Sources Checked

- Childs, "Universal computation by quantum walk", arXiv:0806.1972, Phys. Rev. Lett. 2009: https://arxiv.org/abs/0806.1972

## Verdict

Minor issues. The summary correctly captures the universality theorem, degree-3 unweighted graph, scattering widgets, momentum filtering, and exponential graph size. The main needed fixes are about input-state phrasing and resource precision.

## Line-Anchored Comments

- Lines 17-20: "starting at a designated input vertex"

  This is right only for a computational-basis input. A general input state is encoded as a superposition of wave packets over the corresponding input wires. The graph implements the circuit by scattering those wave packets, not by starting every computation from one vertex.

- Line 27: `2^n` wires

  Correct, and worth emphasizing as the reason this is a universality construction rather than an efficient compiler from circuits to small physical graphs. The graph is succinctly describable from the circuit, but explicitly has size exponential in the number of logical qubits.

- Lines 43-51: gate set summary looks essentially correct.

  Add a note that the crossing widget is a permutation gadget on the wire labels; it implements the controlled-not action in the encoded basis, not a local two-qubit gate acting on two nearby physical qubits.

- Lines 55-59: momentum-filter parameter notation is awkward.

  "`m_d = log Theta(m^2)`" should be rewritten as `m_d = Theta(log m)` filter stages chosen so the unwanted momentum components are suppressed to inverse-polynomial error in the circuit size.

- Lines 81-85: resource table is too vague about total cost.

  Evolution time and success probability are both polynomial, but the final success probability `Omega(1/m^4)` implies polynomial repetition or amplification. State the total overhead explicitly rather than "poly(m) repetitions x O(m^4)".

- Line 97: "degree 3 is optimal"

  Good, with the caveat that the optimality claim is for connected unweighted graph walks in this construction style: degree-2 graphs are disjoint paths/cycles and cannot encode universal scattering computations.

## Missing or Suggested Additions

- Include the distinction between infinite-line scattering analysis and the finite graph used in the actual computation.
- Add a line contrasting this with Feynman-Kitaev history-state universality: both encode computation into time-independent dynamics, but Childs's construction uses graph scattering rather than a clock Hamiltonian.
