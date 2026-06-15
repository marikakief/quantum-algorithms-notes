# Review: PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003)

Source note: [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes]]

## Verdict

Major issues. The protocol explanation is good, but the note appears to conflate rounds and messages. The described protocol is prover -> verifier, verifier -> prover, prover -> verifier: three messages. Calling this `QIP(2)` in standard message-count notation is likely wrong and causes downstream false equalities.

## Line-Anchored Comments

- Lines 11-19: The statement "PSPACE subset QIP(2)" needs correction or an explicit definition of "2-round." Under the common `QIP(k)` convention, `k` counts messages, and the described protocol is a 3-message protocol.

- Lines 11-19: The conclusion "combined with QIP=PSPACE gives QIP(2)=QIP=PSPACE" should be removed unless the notation is verified. Standard results give `QIP = QIP(3) = PSPACE`; the two-message class is a different object.

- Lines 36-54: The quantum transcript-compression protocol is clearly explained. Rename the second block as "messages 2 and 3" or "verifier challenge plus prover response" to avoid the same round/message confusion.

- Lines 56-68: The no-look-ahead explanation is strong. Add that the test detects entanglement/correlation with future random challenges through a uniformity test on challenge registers.

- Lines 70-79: The theorem statement should use the paper's exact message/round terminology, then translate it into the vault's convention.

- Lines 83-91: The comparison table also needs the correction. Classical constant-round IPs and quantum constant-message QIPs should be compared with consistent definitions.

## Missing or Suggested Additions

- Add a notation note: "round" in this paper versus `QIP(k)` message count in later literature.
- Cross-link to Kitaev-Watrous `QIP = QIP(3)` distinctly from the Watrous PSPACE lower-bound construction.
