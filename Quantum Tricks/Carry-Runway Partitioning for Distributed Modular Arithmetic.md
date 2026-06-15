# Carry-Runway Partitioning for Distributed Modular Arithmetic

> **Source:** Gidney and Ekerå, arXiv:1905.09749
> **Tags:** #trick #quantum-arithmetic #distributed-quantum-computing #surface-code #Shor

## What it does

Uses carry-runway segmentation to make large modular-arithmetic additions local to independent pieces, leaving only lookup-address qubits to communicate between pieces.

## The trick

If an $n$-bit register is segmented with [[Oblivious Carry Runway|oblivious carry runways]] every $c_{\mathrm{sep}}$ bits, the actual addition phase is local: each segment can ripple its own carry into a runway without communicating with other segments. In a windowed modular multiplication, the nonlocal part is the QROM lookup that prepares the addend for each segment.

For Gidney--Ekerå's preferred lookup-addition structure, each lookup is addressed by:

- $c_{\exp}$ exponent-window qubits, reused across many lookups;
- $c_{\mathrm{mul}}$ factor-window qubits, streamed through as the multiplication iterates.

With $c_{\exp}=c_{\mathrm{mul}}=5$, only five fresh factor qubits per lookup addition dominate communication. If one physical machine holds one carry-runway segment, the network mainly broadcasts successive factor-window qubits so each machine can perform its local lookup and addition.

For RSA-2048 with lookup additions taking about $37\,\mathrm{ms}$ in the paper's model, broadcasting five qubits per lookup corresponds to a rough bandwidth target of

$$
\frac{5}{37\,\mathrm{ms}} \approx 135\ \text{qubits/s},
$$

rounded in the paper to about $150$ qubits/s per computer.

## When to reach for it

- Arithmetic circuits where carry propagation, not global entangling structure, is the main cross-register dependency.
- Distributed quantum-computing sketches where each node can run a local surface-code patch but total physical qubits per node are capped.
- Layout designs where QROM addresses are much smaller than the data registers being updated.

## Complexity

For $n/c_{\mathrm{sep}}$ pieces, local addition depth is $O(c_{\mathrm{sep}}+c_{\mathrm{pad}})$ rather than $O(n)$. Communication per lookup addition is $O(c_{\mathrm{mul}})$ fresh qubits plus amortised exponent-window qubits. Smaller $c_{\mathrm{sep}}$ gives smaller machines but more pieces, and can slow the lookup if each machine has too few factories.

## Caveat

This is a design pattern, not a finished distributed architecture. The paper gives a surface analysis: two machines of about $11$ million physical qubits each, or possibly eight machines of about $4$ million each with more runtime. It does not solve routing, network error correction, scheduling, or failure recovery for a real distributed run.

## Related notes

- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]
- [[Oblivious Carry Runway]]
- [[Nested Windowed Modular Exponentiation for Shor Arithmetic]]
- [[Coset Representation for Approximate Modular Arithmetic]]
- [[QROM (Quantum Read-Only Memory)]]
