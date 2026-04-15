# Computational Complexity

**Computer Science Fundamentals Series**

Big-O | P vs NP | NP-completeness | Reductions | Approximation | Amortised analysis

*Mid-level software engineer track -- 20 slides*

---

## Table of Contents

1. [Asymptotic Notation -- Big-O and Friends](#slide-02--asymptotic-notation--big-o-and-friends)
2. [Common Complexity Classes](#slide-03--common-complexity-classes)
3. [Comparing Growth Rates](#slide-04--comparing-growth-rates)
4. [Amortised Analysis](#slide-05--amortised-analysis)
5. [Decision Problems and Languages](#slide-06--decision-problems-and-languages)
6. [The Class P](#slide-07--the-class-p)
7. [The Class NP](#slide-08--the-class-np)
8. [NP -- Verifiers and Certificates](#slide-09--np--verifiers-and-certificates)
9. [Polynomial-Time Reductions](#slide-10--polynomial-time-reductions)
10. [NP-Completeness](#slide-11--np-completeness)
11. [The Cook-Levin Theorem](#slide-12--the-cook-levin-theorem)
12. [Classic NP-Complete Problems I](#slide-13--classic-np-complete-problems-i)
13. [Classic NP-Complete Problems II](#slide-14--classic-np-complete-problems-ii)
14. [NP-Hard Problems](#slide-15--np-hard-problems)
15. [P vs NP -- The Open Question](#slide-16--p-vs-np--the-open-question)
16. [Approximation Algorithms](#slide-17--approximation-algorithms)
17. [Parameterised Complexity](#slide-18--parameterised-complexity)
18. [Space Complexity -- PSPACE, L, NL](#slide-19--space-complexity--pspace-l-nl)
19. [Summary and Further Reading](#slide-20--summary-and-further-reading)

---

## Slide 02 -- Asymptotic Notation -- Big-O and Friends

### Why asymptotic notation?

We need a machine-independent way to describe how an algorithm's resource usage (time or space) grows as the input size n increases. Constants and lower-order terms are irrelevant at scale.

### The five notations

| Notation | Meaning | Intuition |
|----------|---------|-----------|
| **O(g(n))** | Asymptotic upper bound | f grows *no faster* than g |
| **Omega(g(n))** | Asymptotic lower bound | f grows *at least as fast* as g |
| **Theta(g(n))** | Tight bound | f grows *at the same rate* as g |
| **o(g(n))** | Strict upper bound | f grows *strictly slower* than g |
| **omega(g(n))** | Strict lower bound | f grows *strictly faster* than g |

### Formal definition (Big-O)

f(n) = O(g(n)) if there exist constants c > 0 and n0 such that for all n >= n0: f(n) <= c * g(n).

> Big-O describes the *worst case* growth rate. It is an upper bound -- saying an algorithm is O(n^2) does not mean it always takes n^2 steps.

---

## Slide 03 -- Common Complexity Classes

### Time complexity hierarchy

| Class | Name | Example algorithm |
|-------|------|------------------|
| **O(1)** | Constant | Hash table lookup |
| **O(log n)** | Logarithmic | Binary search |
| **O(n)** | Linear | Linear search, single pass |
| **O(n log n)** | Linearithmic | Merge sort, heap sort |
| **O(n^2)** | Quadratic | Bubble sort, insertion sort |
| **O(n^3)** | Cubic | Naive matrix multiplication |
| **O(2^n)** | Exponential | Brute-force subset enumeration |
| **O(n!)** | Factorial | Brute-force permutations |

> A problem is *tractable* if it admits a polynomial-time algorithm -- O(n^k) for some constant k. Exponential and factorial problems are *intractable* for large inputs.

---

## Slide 04 -- Comparing Growth Rates

### Concrete numbers at scale

| n | log n | n | n log n | n^2 | 2^n |
|---|-------|---|---------|-----|-----|
| 10 | 3.3 | 10 | 33 | 100 | 1,024 |
| 100 | 6.6 | 100 | 664 | 10,000 | 1.27 x 10^30 |
| 1,000 | 10 | 1,000 | 10,000 | 1,000,000 | -- |
| 10^6 | 20 | 10^6 | 2 x 10^7 | 10^12 | -- |

At n = 100, an O(2^n) algorithm requires more operations than atoms in the observable universe. This is why polynomial vs exponential matters.

### Rules of thumb

- Drop constants and lower-order terms: 5n^2 + 3n + 7 = O(n^2)
- Nested loops multiply: O(n) loop inside O(n) loop = O(n^2)
- Sequential steps add: O(n) + O(n log n) = O(n log n)
- Logarithms: base does not matter -- log_2(n) = Theta(log_10(n))

---

## Slide 05 -- Amortised Analysis

### What is amortised analysis?

Amortised analysis gives the *average* cost per operation over a worst-case sequence of operations. Not the same as average-case analysis (which assumes a probability distribution).

### Three techniques

- **Aggregate method** -- compute total cost of n operations, divide by n
- **Accounting method** -- assign an amortised "charge" per operation; overcharges build credit for expensive operations
- **Potential method** -- define a potential function on the data structure; amortised cost = actual cost + change in potential

### Classic example: dynamic array

A dynamic array doubles its capacity when full. Individual push operations are O(1) except when resizing occurs (O(n) copy). But n pushes cost at most 2n copies total.

*Amortised cost per push: O(1)*

Other examples:

- Splay trees -- O(log n) amortised per operation
- Fibonacci heaps -- O(1) amortised decrease-key
- Increment on a binary counter -- O(1) amortised per increment

---

## Slide 06 -- Decision Problems and Languages

### Decision problems

A *decision problem* asks a yes/no question about an input. Complexity theory focuses on decision problems because they are the simplest to classify.

Examples:

- **PRIME**: "Is n a prime number?" -- Yes/No
- **PATH**: "Does graph G have a path from s to t?" -- Yes/No
- **SAT**: "Is there an assignment that satisfies boolean formula phi?" -- Yes/No

### Languages

A *language* L over alphabet Sigma is a set of strings. A decision problem maps to a language: L = {x in Sigma* : the answer for x is YES}.

A Turing machine M *decides* L if it halts on every input and accepts exactly the strings in L.

> Complexity classes are sets of languages -- they group decision problems by the resources (time, space) required by the Turing machines that decide them.

---

## Slide 07 -- The Class P

### Definition

P is the class of decision problems solvable by a *deterministic* Turing machine in polynomial time: P = UNION over k of DTIME(n^k).

### Why P matters

P captures the intuitive notion of "efficiently solvable" -- problems where a fast algorithm exists.

### Problems in P

| Problem | Algorithm | Time |
|---------|-----------|------|
| Sorting | Merge sort | O(n log n) |
| Shortest path | Dijkstra | O(E + V log V) |
| Primality testing | AKS | O(n^6) (poly in input length) |
| Maximum matching | Edmonds | O(V^3) |
| Linear programming | Interior point | Polynomial |
| 2-SAT | Implication graph | O(n + m) |
| Connectivity | BFS/DFS | O(V + E) |

> Not all polynomial algorithms are fast in practice -- O(n^100) is technically in P but useless. The importance of P is theoretical: it defines the boundary of tractability.

---

## Slide 08 -- The Class NP

### Definition

NP is the class of decision problems solvable by a *nondeterministic* Turing machine in polynomial time.

Equivalently: NP is the class of problems for which a YES answer can be *verified* in polynomial time given a suitable certificate.

### Nondeterminism

A nondeterministic Turing machine can "guess" a solution (explore all branches simultaneously). If any branch accepts in polynomial time, the machine accepts.

### Key relationships

- P is a subset of NP (a deterministic TM is a special case of a nondeterministic TM)
- coNP is the class of problems whose complement is in NP
- It is unknown whether P = NP or whether NP = coNP

> NP does NOT mean "not polynomial". It means "nondeterministic polynomial time". Many NP problems are in P.

---

## Slide 09 -- NP -- Verifiers and Certificates

### The verifier definition

A language L is in NP if there exists a polynomial-time verifier V such that: L = {x : there exists a certificate c with |c| = poly(|x|) and V(x, c) accepts}.

### Examples

| Problem | Certificate | Verification |
|---------|------------|-------------|
| **SAT** | A satisfying assignment | Evaluate formula -- O(n) |
| **CLIQUE** | A set of k vertices | Check all pairs adjacent -- O(k^2) |
| **HAMILTONIAN CYCLE** | A permutation of vertices | Check each edge exists -- O(n) |
| **COMPOSITE** | A non-trivial factor | Perform division -- O(n^2) |
| **SUBSET SUM** | A subset of numbers | Sum and compare -- O(n) |

> The certificate is a "proof" that the answer is YES. The verifier checks this proof efficiently. The hard part is *finding* the certificate, not checking it.

---

## Slide 10 -- Polynomial-Time Reductions

### What is a reduction?

A polynomial-time reduction from problem A to problem B is a polynomial-time computable function f such that: x is in A if and only if f(x) is in B.

Written: A <=_P B ("A reduces to B in polynomial time").

### What reductions tell us

- If A <=_P B and B is in P, then A is in P
- If A <=_P B and A is NOT in P, then B is NOT in P
- Reductions transfer hardness from A to B -- "B is at least as hard as A"

### Classic reduction chain

SAT -> 3-SAT -> CLIQUE -> VERTEX COVER -> HAMILTONIAN CYCLE -> TSP

Each arrow is a polynomial-time reduction. If any of these problems could be solved in polynomial time, so could all of them.

> Reductions are the core tool for proving NP-completeness. To show a new problem X is NP-hard, reduce a known NP-hard problem to X.

---

## Slide 11 -- NP-Completeness

### Definition

A problem L is NP-complete if:

1. L is in NP (solutions can be verified in polynomial time)
2. Every problem in NP reduces to L in polynomial time (L is NP-hard)

NP-complete problems are the *hardest* problems in NP. If any NP-complete problem has a polynomial-time algorithm, then P = NP.

### Significance

- NP-complete problems form the "boundary" of tractability
- Thousands of natural problems are NP-complete
- No polynomial-time algorithm is known for any of them
- Strong evidence (but no proof) that none exists

> Finding a problem NP-complete is practically useful: it tells you to stop looking for an exact efficient algorithm and instead consider approximation, heuristics, or parameterised solutions.

---

## Slide 12 -- The Cook-Levin Theorem

### Statement

SAT is NP-complete. (Cook, 1971; Levin, independently, 1973)

### Why it matters

This was the first problem proven NP-complete. The proof shows that any NP computation can be encoded as a boolean satisfiability instance -- the universal reduction.

### Proof sketch

Given any NP language L decided by a nondeterministic TM M in time p(n):

1. Encode M's computation tableau (all configurations over p(n) steps) as boolean variables
2. Build clauses ensuring: valid start state, valid transitions, accepting final state
3. The formula is satisfiable if and only if M accepts the input
4. The reduction runs in polynomial time -- tableau size is poly(n)

### Consequence

Once SAT is NP-complete, all other NP-completeness proofs reduce from SAT (or from a problem already known to be NP-complete).

---

## Slide 13 -- Classic NP-Complete Problems I

### SAT (Boolean Satisfiability)

Given a boolean formula phi, is there an assignment of variables that makes phi true?

### 3-SAT

SAT restricted to formulas in conjunctive normal form with exactly 3 literals per clause. Still NP-complete (reduced from SAT). Surprisingly, 2-SAT is in P.

### CLIQUE

Given a graph G and integer k, does G contain a complete subgraph of size k?

### INDEPENDENT SET

Given a graph G and integer k, does G contain k pairwise non-adjacent vertices? (Complement of CLIQUE.)

### VERTEX COVER

Given a graph G and integer k, is there a set of at most k vertices that touches every edge?

> These three graph problems are tightly related: S is an independent set if and only if V \ S is a vertex cover. Reductions between them are straightforward.

---

## Slide 14 -- Classic NP-Complete Problems II

### HAMILTONIAN CYCLE

Does graph G contain a cycle visiting every vertex exactly once?

### TRAVELLING SALESMAN (decision version)

Given a weighted complete graph and budget B, is there a tour visiting every city with total cost at most B?

### SUBSET SUM

Given a set of integers S and target t, is there a subset of S summing to exactly t?

### GRAPH COLOURING (k >= 3)

Can the vertices of G be coloured with k colours such that no two adjacent vertices share a colour? NP-complete for k >= 3. Polynomial for k = 2 (bipartiteness check).

### SET COVER

Given a universe U and a collection of subsets, can you cover U with at most k subsets?

> These problems arise constantly in scheduling, logistics, networking, resource allocation, and VLSI design. Their NP-completeness motivates the entire field of approximation algorithms.

---

## Slide 15 -- NP-Hard Problems

### Definition

A problem H is NP-hard if every problem in NP reduces to H in polynomial time. An NP-hard problem does NOT need to be in NP itself.

### NP-hard but not in NP

| Problem | Why not in NP? |
|---------|---------------|
| **Halting problem** | Undecidable -- no TM decides it at all |
| **QSAT (QBF)** | PSPACE-complete; certificate may be exponential |
| **TSP optimisation** | Output is a tour (not yes/no); decision version is in NP |
| **Counting SAT (#SAT)** | Answer is a number, not a yes/no certificate |

### Relationship diagram

```
NP-hard
 |
 |--- NP-complete (NP-hard AND in NP)
 |
 |--- Problems outside NP (undecidable, PSPACE, etc.)
```

> When a problem is NP-hard, no polynomial-time algorithm exists (unless P = NP). This is a stronger statement than NP-completeness for problems outside NP.

---

## Slide 16 -- P vs NP -- The Open Question

### The question

Does P = NP? That is, can every problem whose solution can be verified in polynomial time also be *solved* in polynomial time?

### Millennium Prize

One of the seven Clay Mathematics Institute Millennium Prize Problems. $1,000,000 for a correct proof either way.

### Current consensus

Most complexity theorists believe P != NP, but no proof exists.

### Implications if P = NP

- All NP-complete problems solvable efficiently
- Cryptography based on factoring/discrete log breaks
- Optimisation, scheduling, logistics become trivially solvable
- Mathematical proof discovery becomes automatable

### Implications if P != NP (proven)

- Confirms the existence of inherently hard problems
- Validates the theoretical foundations of modern cryptography
- Justifies the study of approximation, heuristics, and parameterised algorithms

> "If P = NP, then the world would be a profoundly different place. Everyone who could appreciate a symphony would be Mozart." -- Scott Aaronson

---

## Slide 17 -- Approximation Algorithms

### Why approximation?

When exact solutions are NP-hard, we settle for solutions that are provably close to optimal in polynomial time.

### Approximation ratio

An algorithm has ratio rho(n) if for every input of size n: cost(ALG) / cost(OPT) <= rho(n) for minimisation problems.

### Examples

| Problem | Approximation | Ratio |
|---------|--------------|-------|
| **Vertex Cover** | Greedy edge matching | 2-approx |
| **TSP (metric)** | Christofides-Serdyukov | 3/2-approx |
| **Set Cover** | Greedy | O(ln n)-approx |
| **MAX-SAT** | Randomised rounding | 3/4-approx |
| **Knapsack** | FPTAS | (1 + epsilon) for any epsilon > 0 |

### PTAS and FPTAS

- **PTAS** -- Polynomial-Time Approximation Scheme: (1+epsilon)-approx for any epsilon, time polynomial in n (but possibly exponential in 1/epsilon)
- **FPTAS** -- Fully Polynomial TAS: time polynomial in both n and 1/epsilon

> Not all NP-hard problems can be approximated well. TSP (general, non-metric) cannot be approximated within any constant factor unless P = NP.

---

## Slide 18 -- Parameterised Complexity

### The idea

Instead of measuring complexity in input size n alone, we also consider a *parameter* k. A problem is *fixed-parameter tractable* (FPT) if solvable in time f(k) * n^c for some computable f and constant c.

### Why it matters

An O(2^k * n) algorithm is practical when k is small, even if n is large. Classical complexity calls this "exponential" -- parameterised complexity recognises it as tractable.

### FPT examples

| Problem | Parameter k | FPT algorithm | Time |
|---------|------------|---------------|------|
| **Vertex Cover** | Cover size | Bounded search tree | O(2^k * n) |
| **k-Clique** | Clique size | Colour coding | O(2^k * n) |
| **Longest Path** | Path length | Colour coding | O(2^k * n^2) |
| **Treewidth** | Treewidth | Dynamic programming | O(f(k) * n) |

### W-hierarchy

W[0] = FPT subset of W[1] subset of W[2] subset of ... -- analogous to the P/NP hierarchy but parameterised. k-CLIQUE is W[1]-complete, meaning it is unlikely to be FPT.

---

## Slide 19 -- Space Complexity -- PSPACE, L, NL

### Space complexity classes

| Class | Definition | Key problems |
|-------|-----------|--------------|
| **L** | Decidable in O(log n) space (deterministic) | Undirected connectivity (Reingold 2004) |
| **NL** | Decidable in O(log n) space (nondeterministic) | Directed s-t connectivity (PATH) |
| **PSPACE** | Decidable in polynomial space | QSAT, game-playing (generalised chess, Go) |
| **EXPTIME** | Decidable in exponential time | Generalised chess on n x n board |

### Relationships

L is a subset of NL is a subset of P is a subset of NP is a subset of PSPACE is a subset of EXPTIME

By the space hierarchy theorem, L != PSPACE. But whether L = NL or NP = PSPACE remains open.

### Savitch's theorem

NSPACE(f(n)) is a subset of DSPACE(f(n)^2). In particular, NL is a subset of DSPACE(log^2 n). Nondeterminism gives at most a quadratic blowup in space.

### NL-completeness

PATH (directed s-t connectivity) is NL-complete under log-space reductions. It plays the same role in space complexity that SAT plays in time complexity.

> PSPACE captures problems where you can reuse space. Many two-player games are PSPACE-complete -- the game tree is exponential but the space needed to evaluate it is polynomial.

---

## Slide 20 -- Summary and Further Reading

### Key takeaways

- Asymptotic notation (O, Omega, Theta) provides machine-independent growth-rate descriptions
- P captures efficiently solvable problems; NP captures efficiently verifiable problems
- NP-completeness (Cook-Levin, reductions) identifies the hardest problems in NP
- If any NP-complete problem is in P, then P = NP -- the most important open question in computer science
- When exact solutions are intractable, approximation algorithms provide provably good polynomial-time solutions
- Parameterised complexity refines the intractability picture by isolating the source of exponential blowup
- Space complexity (L, NL, PSPACE) adds another dimension to the classification of computational problems

### Recommended reading

| Source | Description |
|--------|------------|
| **Sipser** | *Introduction to the Theory of Computation* -- the standard textbook for complexity theory |
| **Arora & Barak** | *Computational Complexity: A Modern Approach* -- comprehensive graduate-level reference |
| **CLRS** | *Introduction to Algorithms* -- Chapter 34 covers NP-completeness |
| **Garey & Johnson** | *Computers and Intractability* -- the classic NP-completeness catalogue |
| **Complexity Zoo** | [complexityzoo.net](https://complexityzoo.net) -- comprehensive catalogue of complexity classes |
| **Scott Aaronson's blog** | [scottaaronson.blog](https://scottaaronson.blog) -- accessible writing on P vs NP and quantum complexity |
