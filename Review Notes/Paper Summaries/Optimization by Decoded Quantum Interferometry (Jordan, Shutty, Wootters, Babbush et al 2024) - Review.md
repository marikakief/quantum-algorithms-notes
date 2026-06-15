# Review: Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024)

Source note: [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]

## Primary Sources Checked

- Jordan, Shutty, Wootters, Zalcman, Schmidhuber, King, Isakov, Khattar, and Babbush, "Optimization by Decoded Quantum Interferometry," Nature 646, 831-836 (2025), arXiv:2408.08292.
- Khattar et al., "Verifiable Quantum Advantage via Optimized DQI Circuits," arXiv:2510.10967, checked for current resource-estimate context.

## Verdict

Minor-to-major issues. The note is technically ambitious and mostly careful, but the headline claims should separate proven DQI performance, known-classical-algorithm comparisons, and conjectural advantage. It also needs maintenance because the DQI follow-up literature is already moving the resource estimates.

## Line-Anchored Comments

- Line 1: the filename uses 2024, while the publication is Nature 2025. That is fine if the vault uses arXiv posting year, but the title should not make the publication year ambiguous.
- Lines 17-21: the core description is good. "Cannot be dequantized by any relativizing technique" is too strong as written; say the paper gives oracle/relativizing evidence against broad dequantization, not a blanket impossibility theorem.
- Line 19: "superpolynomial speedup over all known classical algorithms" should be immediately qualified as a known-algorithms statement, not a proved separation. The caveat appears at line 108, but it belongs next to the headline.
- Lines 31-41: good max-XORSAT circuit sketch. The decoder cost `T` should be described as reversible decoder cost, including workspace/uncomputation overhead.
- Lines 59 and 122-128: "any advance in decoding theory automatically translates" is slightly too sweeping. It translates only when the decoder can be implemented reversibly with acceptable success probability and cost in the relevant error model.
- Lines 71-77: the OPI performance and resource estimate are useful, but "within plausible fault-tolerant range" should be marked as judgment. The 2025 optimized-DQI follow-up reports much smaller Toffoli counts for related OPI settings, so this section should be versioned.
- Lines 79-89 and 150-152: the max-XORSAT runtime comparison is correctly caveated later, but the first presentation can mislead. Belief-propagation wall-clock time is not a quantum runtime once reversible decoding and error correction are included.
- Lines 95-104: the QAOA comparison is valuable. State explicitly that the "DQI ceiling" mentioned is for classical decoders under the analyzed setting.
- Lines 106-118: excellent caveat section. Move the first two caveats closer to the opening.
- Lines 146-156: the assessment is thoughtful, but "the OPI result is real" should be phrased more precisely: the quantum algorithmic improvement over Prange is real; the classical hardness assumption remains open.
- Line 166: the optimized-DQI follow-up is now central context, not just a cross-link. Also consider adding the 2026 DQI extensions if the note keeps a live literature section.

## Missing or Suggested Additions

- Add a "status as of May 2026" box: Nature paper result, optimized-DQI resource follow-up, and any newer DQI variants should be separated from the original paper summary.

