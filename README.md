<!-- solve-problem:bf8c29d7-21ff-4cc6-8a94-9d2338790eb0 -->
# Superlinear unrestricted Boolean circuit lower bounds

A [Tangle Solve problem](https://solve.tangle.tools/problems/bf8c29d7-21ff-4cc6-8a94-9d2338790eb0).

## What is the problem?

Prove that there exists an explicit Boolean function f : {0,1}^n → {0,1} (or {0,1}^n → {0,1}^m) that requires superlinear size in **unrestricted** Boolean circuits over the standard basis {AND, OR, NOT} (or any complete basis) with fan-in 2. That is, find f computable in polynomial time (a uniformity/explicitness condition, since we need to actually compute f on input x in poly(n) time) such that any circuit computing f has size ω(n) — ideally n·k for an unbounded constant factor, or n^{1+ε}.

Formal setting:
- Circuit model: directed acyclic graph; each internal node is a binary AND/OR/NOT gate; size = number of gates; depth unrestricted.
- "Explicit" typically means: f_n is computable by a deterministic Turing machine in poly(n) time (or at least f is in E or a mild uniform class).
- Constraint: the circuit family computing f must be arbitrary (nonuniform), so the lower bound must hold against all circuits.

Ambiguity note: "superlinear" most reasonably means size ≥ n·g(n) for some unbounded g; the weakest meaningful target is 5n or any fixed constant >1 times n (Wegener's survey frames it this way). Since n inputs must be "read," the trivial lower bound is ~n; the challenge is beating it by any unbounded factor. Note that in basis {AND,OR,NOT}, NOT gates can be absorbed into inputs at only a linear cost, and the true counting lower bound is Ω(n/log n) per function bit via Shannon counting — so the goal is to exceed n by ω(1)·n/log n-type slack (see Warnings).

## Why does it matter?

This is the weakest open lower bound for unrestricted circuits: we cannot prove even 5n for an explicit function, while counting shows almost every function needs ~2^n/n gates. Crossing superlinear would be the first breach of the "linear barrier" and is a prerequisite for any route to NP ⊄ P/poly. Practically, it sets provable limits on circuit compression in hardware synthesis and provides candidate explicit hard functions for cryptography (e.g., foundations of one-way function hardness against small circuits). Virtually all modern circuit-lower-bound technology (natural proofs, rank/accuracy methods) is calibrated against this benchmark.

## What's already known?

- **Counting (Shannon 1949, Lupanov):** Almost every function requires (2^n/n)-size circuits; Lupanov proved O(2^n/n) suffices, so the counting bound is tight — but such functions are nonexplicit.
- **Restricted models:** Strong, often exponential, lower bounds are known for bounded depth (Håstad's switching lemma: parity needs exp(depth) size at depth d), monotone circuits (Razborov: clique needs n^{Ω(log n)}; exponential for some functions via approximation), bounded fan-in formulas (superquadratic), and read-once branching programs. All these techniques fail for unrestricted circuits.
- **Barriers:** Natural proofs (Razborov–Rudich) — any "natural" largeness/constructivity proof would break pseudorandom functions. Algebrization (Aaronson–Wigderson) and relativization block most diagonalization/lifting arguments.
- **Best unconditional explicit results:** (n^{3-ε})-ish bounds only for functions with many output bits in restricted or arithmetic models; for single-output unrestricted circuits, no explicit ω(n) bound exists. Viola (2017) surveys candidate hard functions (e.g., MCSP, matrix multiplication rigidity) and why current methods fail.
- **Approaches tried:**ACC lower bounds for NEXP (Williams) — doesn't extend to unrestricted circuits; approached via SAT algorithms + circuit lower bounds; rigidity and complexity of linear maps; all stuck at restricted bases/depth.

## What would count as solving it?

Any of:
1. **Upper-bound-free lower bound:** An explicit family {f_n} (each f_n computable in poly(n) time) with a proof that C(f_n) ≥ c·n for arbitrarily large constant c (or n·ω(1), or n^{1+ε}).
2. **Matching-bounds variant:** Matching Lupanov's counting lower bound with an explicit function would be the ultimate result, but any superlinear explicit bound resolves the census problem.
3. **Intermediate milestones a referee would still value:** ω(n) for circuits of bounded depth d (already known), or for {AND,OR,NOT} circuits with restricted negations; but these do NOT solve the problem — the statement requires unrestricted circuits.
A referee accepts: a rigorous size lower bound of ω(n) for a function with a stated explicitness (uniformity) condition.

## How to start

1. Read Viola (2017), *Selected Challenges in Computational Lower Bounds* (arXiv:1712.01230) — it directly targets the "5n lower bound" framing and lists candidate functions; then Wegener's book *The Complexity of Boolean Functions* for the classical toolkit.
2. Understand why Shannon counting fails to be made explicit: the hard functions are information-theoretically dense; explicitness forces structure that current methods can't exploit negatively.
3. Consider restricted stepping stones: prove ω(n) for formulas or for circuits with limited negation complexity (known results exist — check where exactly the frontier is), then attempt to lift.
4. Study the natural-proofs barrier (Razborov–Rudich 1997) early to avoid wasting months on doomed approaches; consider non-natural strategies such as Williams's "algorithmic method" (SAT algorithms ⇒ lower bounds) and ask whether it can be pushed past ACC.

Main obstacle: every known lower-bound technique either applies only to restricted models or is ruled out by natural proofs/algebrization.

## Key references

- Shannon, C. E. (1949), "The Synthesis of Two-Terminal Switching Circuits," Bell System Technical Journal. Proves by counting that almost all Boolean functions of n variables require exponentially many relay contacts — the origin of the counting lower bound. No stable link; available via JSTOR/IEEE.
- Viola, E. (2017), "Selected Challenges in Computational Lower Bounds," SIGACT News / arXiv:1712.01230. A survey of open lower-bound problems, explicitly highlighting that even a 5n lower bound for explicit functions in unrestricted circuits is open, and reviewing candidate techniques. https://arxiv.org/abs/1712.01230
- Razborov, A. and Rudich, S. (1997), "Natural Proofs," J. Comput. Syst. Sci. 55(1). Proves that any broadly applicable ("natural") circuit lower-bound proof would yield fast algorithms distinguishing pseudorandom functions, explaining the field's stagnation. https://www.cs.umd.edu/~gasarch/BLOGPAPERS/natural.pdf
- Håstad, J. (1986), "Computational Limitations of Small-Depth Circuits." Proves exponential lower bounds for parity in constant-depth circuits — the exemplar of success in restricted models. (MIT Press.)
- Wegener, I. (1987), *The Complexity of Boolean Functions*. Standard textbook covering Lupanov's O(2^n/n) upper bound and all classical lower-bound techniques for restricted models.
- Williams, R. (2014), "Nonuniform ACC Circuit Lower Bounds," JACM 61(1). Proves NEXP ⊄ ACC via the algorithmic method; the closest modern progress toward unrestricted bounds. https://arxiv.org/abs/1203.1899

## Warnings

- **Negations are nearly free:** XOR-type arguments don't give size lower bounds here; Lupanov's theorem means the true complexity landscape is subtle — Shannon counting with the full basis gives only ~n/log n per output, so "superlinear" means beating the model's baseline carefully. State the basis and size measure (gates vs. wires) precisely; note NOT gates of fan-out 1 can be pushed to inputs with linear overhead, changing constants.
- **Single output vs. many outputs:** Some claimed "superlinear" results concern functions with n outputs or formulas — these do not solve the census problem as stated.
- **Don't re-derive restricted-model bounds** (monotone, constant-depth, formulas); these are well-trodden and their techniques provably (or conjecturally) don't lift.
- **Natural-proofs trap:** Any argument that (a) applies to most functions and (b) is efficiently checkable is doomed unless it exploits a hardness assumption.
- Ambiguity: the census does not specify fan-out or whether size counts wires; fix conventions upfront.

---
*`ALGO-000033` · unverified*

## Acceptance guidance

Any of:
1. **Upper-bound-free lower bound:** An explicit family {f_n} (each f_n computable in poly(n) time) with a proof that C(f_n) ≥ c·n for arbitrarily large constant c (or n·ω(1), or n^{1+ε}).
2. **Matching-bounds variant:** Matching Lupanov's counting lower bound with an explicit function would be the ultimate result, but any superlinear explicit bound resolves the census problem.
3. **Intermediate milestones a referee would still value:** ω(n) for circuits of bounded depth d (already known), or for {AND,OR,NOT} circuits with restricted negations; but these do NOT solve the problem — the statement requires unrestricted circuits.
A referee accepts: a rigorous size lower bound of ω(n) for a function with a stated explicitness (uniformity) condition.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/bf8c29d7-21ff-4cc6-8a94-9d2338790eb0?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000033.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000033)
