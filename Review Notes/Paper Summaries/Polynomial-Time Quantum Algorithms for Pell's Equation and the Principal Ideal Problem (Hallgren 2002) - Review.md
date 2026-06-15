# Review: Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002)

Source note: [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]

## Primary Sources Checked

- Hallgren, "Polynomial-time quantum algorithms for Pell's equation and the principal ideal problem", STOC 2002 / JACM 54 (2007).
- Jozsa, "Notes on Hallgren's efficient quantum algorithm for solving Pell's equation", arXiv:quant-ph/0302134.

## Verdict

Major issues. The regulator/PIP algorithm is described well, but the note overclaims the corollaries and the "strictly harder than Shor" comparison. It should distinguish Hallgren 2002 from later class-group/unit-group algorithms.

## Line-Anchored Comments

- Lines 11-17: good explanation of why the regulator is the target rather than writing the fundamental solution outright.
- Lines 19-24: accurate high-level contribution.
- Lines 30-41: the infrastructure/principal-cycle explanation is good.
- Lines 45-57: weak periodicity and discretization are clearly described.
- Lines 62-64: line 64 is too broad for this source. Hallgren 2002/2007 solves Pell/regulator and PIP for real quadratic fields; broader class group/unit group algorithms belong to later Hallgren/Schmidt-Vollmer work and need separate qualification.
- Lines 70-75: the comparison is risky. Known classical algorithms for Pell/regulator have worse asymptotic exponents than NFS factoring, but "strictly harder" is a complexity-theoretic claim the note has not justified. Say "harder by the best known classical algorithms" or "at least as hard as factoring under known reductions" if that is the intended point.
- Lines 79-88: caveats are good and should be moved near the key-results section.
- Lines 95-98: reusable ideas are well chosen.

## Missing or Suggested Additions

- Add a sentence distinguishing the narrow regulator `R+`/regulator conventions from later Schmidt's narrow-regulator treatment.
- Include a precise statement of what PIP output is certified and how success probability is amplified.

