# Review: Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024)

Source note: [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]]

## Primary Sources Checked

- Berry et al., "Quantum Simulation of Realistic Materials in First Quantization Using Non-local Pseudopotentials", arXiv:2312.07654 / npj Quantum Information (2024).
- Cross-checked against Su et al. 2021 for the predecessor first-quantized block encoding.

## Verdict

Minor issues. The note is detailed and mostly accurate. The main edits should temper a few architectural claims and make the cost tradeoff around `lambda_nonloc` even more explicit.

## Line-Anchored Comments

- Line 21: "first complete block encoding" is plausible in the context of realistic GTH nonlocal pseudopotentials, but the phrase should specify "for this first-quantized plane-wave/GTH setting" to avoid dismissing unrelated pseudopotential work.
- Line 23: "exponential improvement" should be explained as exponential in the address-register size (`N` table entries versus `polylog N` arithmetic), not an exponential improvement in the physical problem size in the complexity-theory sense.
- Lines 23 and 56-72: the arithmetic-over-QROM and shared-exponential story is very good. It is the strongest conceptual part of the note.
- Line 25: `O(10^14)` Toffolis is right as an order-of-magnitude summary for the materials examples, but the table later spans `6e13` to `1.1e15`. Say "roughly `10^14`-`10^15` depending on material" if summarizing.
- Line 27: "spacetime volume comparison favours first quantization" should be tied to the LNO comparison and the physical-resource assumptions. It is not a universal dominance statement over all second-quantized material algorithms.
- Lines 52-55: this is a key tradeoff. Breaking up `l,i,j` and using box-level bounds is exactly why the block encoding is cheap but `lambda_nonloc` is huge. The note captures this, but it should be echoed in the opening summary.
- Line 104: "complete pseudopotential" is better than "full Hamiltonian" here. The paper's point is inclusion of the omitted angular-momentum projectors; no finite simulation captures every material effect.
- Line 150: "Angular momentum error: None" is too absolute. Better: "No omission of the nonlocal angular-momentum projector terms considered in this comparison."
- Line 160: the large-core pseudopotential caveat is important and should be moved closer to the headline resource estimates; it materially affects accuracy and electron count.

## Missing or Suggested Additions

- Add one sentence comparing raw Toffoli count, logical qubits, and spacetime volume separately. The note currently moves between those metrics quickly.
- Add a caveat that pseudopotential accuracy is now part of the physics approximation stack, distinct from block-encoding error and QPE error.

