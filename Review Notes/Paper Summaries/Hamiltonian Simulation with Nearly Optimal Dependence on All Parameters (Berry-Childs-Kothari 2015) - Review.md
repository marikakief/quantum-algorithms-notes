# Review: Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015)

Source note: [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]]

## Primary Sources Checked

- Berry, Childs, and Kothari, "Hamiltonian simulation with nearly optimal dependence on all parameters", FOCS 2015 / arXiv:1501.01715.

## Verdict

Major issues. The Bessel-linearization story is strong, but the note misstates the later QSP benchmark in a way that reverses the precision logarithm and obscures the actual remaining upper/lower gap.

## Line-Anchored Comments

- Lines 9-15: good statement of what this paper combines: walk sparsity scaling plus LCU precision scaling.
- Lines 23-25: the arcsin/eigenphase obstruction is explained well.
- Lines 33-44: the Bessel generating-function explanation is good. It should note that signs/phases are chosen so the same polynomial works on the two relevant walk eigenphase branches.
- Lines 60-65: the table correctly gives this paper's upper bound and lower bound.
- Lines 81-85: the lower-bound combination is useful. The "sum vs product" upper/lower gap is exactly the right caveat.
- Lines 91-98: good explanation of the `tau`-`epsilon` tradeoff.
- Line 124: the QSP comparison is wrong. Low-Chuang/QSP Hamiltonian simulation has an additive precision term of order `log(1/epsilon)/loglog(1/epsilon)` in the usual query model, not `log(1/epsilon) loglog(1/epsilon)`.
- Lines 124-125: the note says the `loglog` denominator "persists" as a limitation. The sharper statement is: this paper's upper bound multiplies `tau` by a precision factor, while QSP makes time and precision additive. The precision lower bound itself has the `log/loglog` form.
- Line 127: "QSP-based methods ... don't need the walk construction" is misleading. Qubitization/QSP uses a walk or signal unitary; it just packages it in a cleaner block-encoding geometry rather than this Szegedy+Bessel construction.

## Missing or Suggested Additions

- Add the normalized sparse parameter `tau=d||H||_max t` in every comparison table; otherwise this paper is too easy to confuse with BCCKS 2014's `d^2` parameter.
