# Review: The Learnability of Quantum States (Aaronson 2006)

Source note: [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]]

## Verdict

Minor-to-major issues. The note is strong on the fat-shattering/QRAC proof and correctly positions the paper as the offline predecessor of shadow tomography. The communication-complexity application is misstated in a way that could mislead readers about the `M` parameter.

## Line-Anchored Comments

- Lines 19-24: Good distinction between probability-value and single-bit models. Add how many copies of `rho` are consumed to produce a probability-value estimate, since the learning theorem treats those estimates as training labels.

- Lines 35-56: The theorem statements are useful. The note should be more consistent about whether `epsilon` denotes the fraction of bad measurements, the target additive error, or empirical-estimation accuracy. The paper uses several parameters, and the summary compresses them aggressively.

- Lines 70-80: The QRAC proof of the fat-shattering bound is the best part of the note. It is correct and should be retained.

- Lines 86-88: Good computational caveat. Add that even writing a general density matrix hypothesis is exponential; the SDP is not merely slow, it lives in an exponentially large representation.

- Lines 90-98: The one-way communication paragraph has a parameter problem. If `M` is Bob's input length, the theorem's factor is in that length (up to logs), not the size of Bob's input domain `2^M`. The sentence "The M factor comes from the domain size of Bob's measurements" is wrong or at least dangerously ambiguous.

- Lines 102-115: The advice-class summary is broadly right, but "stores a classical approximation sigma" should be worded carefully. The advice is a classical witness/hypothesis sufficient for the verifier's measurements, not a full explicit density matrix in ordinary tomography form.

- Lines 119-135: The comparison to shadow tomography is good. Add that Aaronson 2018 handles a finite known list of measurements and uses joint/gentle measurements, while the 2006 result is distributional PAC learning.

## Missing or Suggested Additions

- Add a notation box for `gamma`, `epsilon`, `eta`, and `delta`.
- Rewrite the communication-complexity paragraph using the original theorem's notation to avoid confusing input length with number of possible inputs.
