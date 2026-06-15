# Review: Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015)

Source note: [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]

## Primary Sources Checked

- Grassl, Langenberg, Roetteler, and Steinwandt, "Applying Grover's algorithm to AES: quantum resource estimates", arXiv:1512.04965 (2015).

## Verdict

Minor issues. The note correctly treats the paper as an early logical-resource estimate, not a complete attack cost. The key improvement would be to distinguish black-box Grover iteration count, logical Clifford+T counts, and fault-tolerant wall-clock costs even more explicitly.

## Line-Anchored Comments

- Lines 11-26: the Grover predicate and multiple plaintext-ciphertext pairs are well framed. The uniqueness estimate is heuristic and should stay labelled that way.
- Lines 20-21: good cost-model statement. It should be repeated near the headline T counts: these are logical circuits with no geometry or error correction.
- Lines 30-32: good assessment. "Depth is the real blocker" is right in the logical model, but in a fault-tolerant model T-factory throughput and routing can dominate.
- Lines 36-64: the diffusion and comparison costs are useful. Make clear that these are per Grover iteration overheads, not one-time setup.
- Lines 69-111: the S-box discussion is strong. The finite-field inverse is the dominant non-Clifford component.
- Lines 112-135: the key-schedule storage tradeoff is explained well. This is a reversible checkpointing story, not just a table of qubits.
- Lines 148-156: the compute/compare/uncompute oracle composition is correct. Note that parallel AES boxes reduce depth but multiply qubits and gate count.
- Lines 184-193: the AES-192 binary/decimal inconsistency caveat is good. Keep the decimal source value if the table extraction is suspect.
- Lines 212-217: excellent limitations. They are the right guardrail against treating the estimates as current hardware forecasts.

## Missing or Suggested Additions

- Add one sentence saying that Grover iterations are sequential because amplitude amplification rotates in a two-dimensional subspace; parallelizing oracle internals does not parallelize the iteration count.
- If this note is later compared to SHA resource estimates, use the same vocabulary: logical T count/depth versus surface-code cycles/logical-qubit-cycles.

