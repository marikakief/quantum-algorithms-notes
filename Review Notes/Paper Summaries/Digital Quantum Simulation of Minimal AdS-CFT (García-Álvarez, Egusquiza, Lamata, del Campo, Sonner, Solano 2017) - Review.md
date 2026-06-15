# Review: Digital Quantum Simulation of Minimal AdS-CFT (Garcia-Alvarez-Egusquiza-Lamata-del Campo-Sonner-Solano 2017)

Source note: [[Digital Quantum Simulation of Minimal AdS-CFT (García-Álvarez, Egusquiza, Lamata, del Campo, Sonner, Solano 2017) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/1607.08560
- PRL DOI linked in the source note.

## Verdict

Major issues. The high-level description is recognizable, but the resource scaling discussion contains internal contradictions and overconfident holography language. The note should be rewritten around the paper's actual Trotter/SYK proposal, with a much cleaner derivation of the `N` scaling.

## Line-Anchored Comments

- Lines 15-18: The holographic motivation is too strong. SYK is connected to nearly-AdS2/JT-like low-energy physics in a specific large-`N`, low-temperature regime; the paper does not experimentally or algorithmically establish a general "holographic dual to two-dimensional dilaton gravity."

- Lines 31-36: Good broad list of contributions. Keep "proposal" visible: this is an algorithmic/digital-simulation proposal, not an experimental demonstration.

- Lines 46-49 and 74-79: The note first says each Majorana quartic maps to a Pauli string on `<=4` qubits "with JW tails," then later says the JW string length is `O(N)`. The correct qubit operator is generally a Pauli string with parity tails; its support is not just four qubits.

- Lines 60-67: This section is internally inconsistent. It first claims a Frobenius/norm calculation giving `N^{10}`, then notes the naive coefficient sum gives something much smaller, then says the `N^{10}` comes from commutators. Replace this with the paper's actual Trotter-error bound and define exactly which norm/term count produces each factor.

- Lines 81-90: OTOC measurement generally needs an interferometric or echo protocol and careful treatment of forward/backward evolutions and inserted operators. The five-step list is too schematic to be a measurement circuit.

- Lines 100-104: The comparison to Babbush-Berry-Neven should be stated as a later algorithmic improvement in asymptotic gate complexity, not a direct fixed-constant experimental replacement.

- Lines 113-115: Check the concrete `N=8` and `N=20` gate estimates against the paper. As written, "per Trotter step" appears inconsistent with the earlier `O(N^5)` per-step count.

## Missing or Suggested Additions

- Add a precise parameter convention: `N` Majorana modes, `N/2` qubits, `binom(N,4)` terms, coupling variance, and dimensionless time `Jt`.

- Add a one-line status note: later asymmetric qubitization and SOSSA substantially supersede this Trotter approach for fault-tolerant resource estimates.

