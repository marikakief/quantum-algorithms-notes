# Tidy Quantum Subroutine via Boost-Copy-Uncompute

> **Source:** Bennett, Bernstein, Brassard & Vazirani, arXiv:quant-ph/9701001 (1997)
> **Tags:** #trick #subroutine #BQP #uncomputation #error-reduction

## What it does
Converts a bounded-error quantum computation into a "tidy" one that leaves no garbage — just the input and the answer. This makes BQP subroutines safe for coherent use inside larger quantum algorithms, proving BQP$^{\text{BQP}}$ = BQP.

## The trick

A BQP machine $M$ computing $f(x)$ produces a final superposition $\sum_i \alpha_i |y_i\rangle$ where each $y_i$ encodes $f(x)$ plus some garbage. If you copy $f(x)$ and reverse $M$, the garbage prevents proper interference — different garbage strings don't cancel.

**Fix:**
1. **Boost** $M$'s success probability to $1 - \epsilon$ by running $k = O(\log 1/\epsilon)$ independent copies and taking majority vote.
2. **Copy** the answer bit to a clean ancilla register.
3. **Run $M$ in reverse** (every halting QTM can be reversed with 5× slowdown).

Since $(1-\epsilon)$ of the superposition has the correct answer, the copy is the same across almost all branches. After reversal, the final state is within $\sqrt{\epsilon}$ of $|x\rangle|f(x)\rangle$. The garbage has been uncomputed.

**Cost:** $O(T \cdot \text{poly}(\log 1/\epsilon))$ where $T$ is $M$'s running time.

## When to reach for it

- Composing quantum subroutines: when subroutine $A$ must be called coherently by algorithm $B$
- Building oracle machines from BQP computations
- Any situation where a quantum computation's intermediate state must be cleaned up for subsequent interference to work
- Justifying the "quantum oracle" abstraction in complexity-theoretic arguments

## Complexity
Overhead is polynomial in $\log(1/\epsilon)$ and linear in $T$. The reversal adds a constant factor of 5.

## Caveat
- Only works when the subroutine's answer is discrete (a bit or short string) — you need a clean copy step.
- The boosting via majority vote requires running $M$ multiple times on independent copies, so space usage grows by factor $k$.
- In practice, the 5× reversal overhead and boosting copies make this expensive. The result is mainly of theoretical importance for complexity-class closure.

## Related notes
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]
- [[Standard Amplitude Amplification]] — another use of boost-then-uncompute ideas
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
