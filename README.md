<!-- solve-problem:4cd27484-be91-4e4c-8da8-817327205b73 -->
# Polynomial-time unknot recognition

A [Tangle Solve problem](https://solve.tangle.tools/problems/4cd27484-be91-4e4c-8da8-817327205b73).

## What is the problem?

Given a knot diagram D (a planar 4-valent graph with over/under crossing information) with n crossings, decide in time polynomial in n whether D represents the unknot — i.e., whether the knot it depicts is ambient-isotopic to the standard circle in S³.

Formal setting: input is a diagram via, e.g., a Gauss code or PD code of length O(n). The decision problem UNKNOT is in NP (via Hass–Lagarias–Pippenger's normal surface certificates: a spanning disk with bounded bit complexity) and in coNP (via Lackenby's 2016/2021 result: a certificate of knottedness — a sequence of Reidemeister moves of polynomially bounded length). The open question: is UNKOT in P? That is, does there exist a deterministic algorithm with worst-case running time poly(n)?

Key subtlety: running time must be polynomial in the number of crossings, not in the size of any certificate or spanning surface. Existing normal-surface algorithms have running times polynomial in the *exponential-size* bounds on triangulations/certificates, i.e., exponential (Hass–Lagarias–Pippenger gave ~n^{O(n)} or exponential bounds; Lackenby improved various bounds to quasipolynomial/n^{c log n} for certain problems, but not polynomial for recognition).

Note ambiguity: one may ask for (a) exact polynomial-time decision, (b) randomized polynomial time, or (c) polynomial time in the bit model for triangulation-based input. The standard interpretation is (a) with diagram input.

## Why does it matter?

UNKNOT is the most basic decision problem in 3-manifold topology and a benchmark for whether low-dimensional topology is "algorithmically tame." A polynomial-time algorithm would likely introduce a new technique beyond normal surface theory (which underlies all known algorithms for 3-manifold problems), with fallout for recognizing S³, link triviality, and knotted graph problems. Practically, DNA topology labs routinely need to distinguish knotted from unknotted circular DNA, and current exponential worst-case algorithms are only heuristically fast. The problem also sits in NP ∩ coNP, and resolving it one way or another (e.g., NP-completeness of a related problem, or a poly algorithm) would clarify the complexity landscape of topological problems.

## What's already known?

- **Decidability and complexity upper bounds:** Hass, Lagarias, and Pippenger (1999) proved UNKNOT ∈ NP by showing a diagram of the unknot bounds a spanning disk whose normal surface coordinates have polynomially bounded bit length; their algorithm via normal surfaces runs in exponential time (roughly 2^{O(n)}).
- **coNP membership:** Lackenby (2016, announced; detailed in the 2021 paper "The efficient certification of knottedness and Thurston norm") showed that any diagram of a nontrivial knot can be reduced to a diagram with ≤ 3 crossings of trefoil-type gadget — more precisely, if a diagram with c crossings represents a nontrivial knot, there is a sequence of Reidemeister moves of length polynomial in c taking it to a diagram with at most two crossings fewer / a minimal diagram, implying knottedness has a polynomial-size certificate. Hence UNKNOT ∈ NP ∩ coNP.
- **Reidemeiter move bounds:** Coward and Lackenby showed an unknot diagram can be unknotted via exponentially many (2^{O(n)}) Reidemeister moves (via Dynnikov / arc presentations); Hayashi, and later Lackenby (2015, "A polynomial upper bound on Reidemeister moves") showed simplifying certain diagram classes needs only polynomially many moves, but not for arbitrary unknot diagrams — the general bound remains exponential (Dynnikov's work on arc presentations gives a combinatorial simplification that is fast in practice but lacks polynomial worst-case bounds).
- **Practical/heuristic algorithms:** Dynnikov's monodromy/arc-presentation approach untangles any unknot monotonically but without proven polynomial bounds; SnapPy-style methods use hyperbolic structures and are excellent in practice.
- **Hardness context:** UNKNOT is not known NP-hard (and NP ∩ coNP membership makes this unlikely without PH collapse). Agol, Hass, Thurston proved that counting 3-colorings / computing certain invariants is #P-hard; vertices of the normal surface cone can be exponential.

Why approaches fall short: normal surface algorithms enumerate over a possibly exponential set of fundamental surfaces; the obstruction is that NP membership gives short *certificates* found by nondeterminism, not efficient search, and no structure (like LP duality or symmetry) is known to prune the search.

## What would count as solving it?

Primary target: **A deterministic algorithm that, given any n-crossing diagram D, decides whether D is the unknot in time n^{O(1)}, with a proof of correctness and worst-case running time.** The referee-acceptable criterion is a rigorous complexity analysis in the bit model (accounting for coefficient growth in linear algebra or normal surface coordinates).

Accepted variants (any counts as major progress; the first fully solves the problem):
1. Polynomial-time deterministic algorithm (full solution).
2. Polynomial-time randomized (BPP) algorithm.
3. A proof that no such algorithm exists is essentially out of reach; conditional hardness results would also be notable but are not the target.
4. Intermediate milestones: quasipolynomial time in worst case (improving on exponential), or polynomial time for restricted diagram classes with proof.

## How to start

1. **Read Hass–Lagarias–Pippenger (1999), Sections on spanning disk normal surface bounds**, to internalize why NP certificates exist and exactly where the exponential blowup enters (enumerating fundamental normal surfaces in a triangulation with t tetrahedra).
2. **Read Lackenby (2021), "The efficient certification of knottedness and Thurston norm"** — the coNP certificates are the dual side; since UNKNOT ∈ NP ∩ coNP, ask whether the two certificate types can be combined into a search via LP-type methods or self-reduction (as with LP duality or prime testing).
3. **Study Dynnikov's arc-presentation simplification** (monotonic simplification of unknots): the lack of a polynomial bound comes from arc index growth; try to bound arc index changes under grid moves combinatorially.
4. **Explore alternative certificates:** Kuperberg's certificate that a 3-manifold is S³ (via triangulation + Heegaard splitting, related to the Virtual Haken / recognition-of-S³ program) or homology/gauge-theoretic invariants; ask if there is a polynomial-size *uniquely determined* certificate enabling binary search / Newton-type iteration.

Main obstacle: normal surface coordinates of the solution can be exponentially large in number even when each has polynomial bit length; no convexity or duality structure is known to make the search efficient.

## Key references

- Joel Hass, Jeffrey Lagarias, Nicholas Pippenger, "The Computational Complexity of Knot and Link Problems," *Journal of the ACM* 46 (1999), 185–211. Proves UNKNOT ∈ NP via polynomially-bounded normal-surface spanning disk certificates and gives the first exponential-time algorithms for knot problems. https://doi.org/10.1145/301970.301971
- Marc Lackenby, "The efficient certification of knottedness and Thurston norm," *Advances in Mathematics* 387 (2021), 107796. Shows knottedness admits polynomial-size certificates (via polynomially-bounded Reidemeister move sequences to trefoil-containing subdiagrams), placing UNKNOT in coNP. https://arxiv.org/abs/1604.00290
- Marc Lackenby, "A polynomial upper bound on Reidemeister moves," *Geometry & Topology* 20 (2016). Proves polynomially many Reidemeister moves suffice to reduce diagrams lacking specific Gordian obstructions — the tool behind the coNP result. https://arxiv.org/abs/1302.0180
- Ivan Dynnikov, "Arc-presentations of links: monotonic simplification," *Fundamenta Mathematicae* 190 (2006). Gives a monotonic untangling procedure for unknots via arc presentations that is fast in practice but lacks polynomial worst-case bounds. https://arxiv.org/abs/math/0208153
- (Optional context) Sergey Kuperberg, "Knottedness is in NP, modulo GRH," *Advances in Mathematics* 256 (2014). Alternative certificate framework for knottedness related to S³ recognition. https://arxiv.org/abs/1112.0845

## Warnings

- Do not conflate "polynomially many Reidemeister moves" with "polynomial-time algorithm": enumerating the right move sequence is the hard part.
- The Hass–Lagarias–Pippenger NP result bounds the *bit length* of the spanning disk's normal coordinates, not the number of candidate surfaces — exhaustive search over fundamental surfaces is where exponential time enters.
- Heuristic algorithms (Dynnikov, SnapPy) solve random instances instantly; any claimed poly algorithm must handle adversarial hard families (e.g., iterated twist knots, weaving knots) — benchmark against these.
- Ambiguity in input model: Gauss codes must be realizable diagrams; PD codes avoid this. State your input convention up front.
- NP-hardness of UNKNOT itself would imply NP = coNP-type collapse given coNP membership — do not chase that direction without awareness of the barrier.

---
*`ALGO-000207` · unverified*

## Acceptance guidance

Primary target: **A deterministic algorithm that, given any n-crossing diagram D, decides whether D is the unknot in time n^{O(1)}, with a proof of correctness and worst-case running time.** The referee-acceptable criterion is a rigorous complexity analysis in the bit model (accounting for coefficient growth in linear algebra or normal surface coordinates).

Accepted variants (any counts as major progress; the first fully solves the problem):
1. Polynomial-time deterministic algorithm (full solution).
2. Polynomial-time randomized (BPP) algorithm.
3. A proof that no such algorithm exists is essentially out of reach; conditional hardness results would also be notable but are not the target.
4. Intermediate milestones: quasipolynomial time in worst case (improving on exponential), or polynomial time for restricted diagram classes with proof.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4cd27484-be91-4e4c-8da8-817327205b73?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000207.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000207)
