# The CHAT Researcher's Bible
### A condensed proof you can hold in your head, a map of the surrounding country, and an honest list of where the dragons live

*For a reader who already knows what Collatz is, what 3-adic digits are, and what a commutator does, and who wants the structure of the proof without the scaffolding.*

**Tags.** **[Held]** means proved (the formal Lean 4 development, or a short derivation from proved facts). **[Derived]** means worked out by hand, unchecked. **[Belief]** is judgment. **[Memory]** is literature recalled from memory, to verify before citing. **[Dragon]** marks expected hard resistance.

---

## 1. The problem in one page

**The mismatch.** Halving is invisible to 3-adic information, because 2 is a unit in Z₃. The step 3n+1 is invisible to 2-adic information. The conjecture lives where the two collide.

**The backward tree.** From a number x you can go back by *doubling* (an invertible reshuffle of base-3 digits, nothing lost) or by *undoing a tripling*, which means dividing by 3. Division by 3 chops a trailing 0 off a base-3 expansion, so the window of known digits slides and one digit is lost. So the ancestor tree is a branching process on the last base-3 digits: one lossless road, one road that forgets.

**The question CHAT answers.** Is the ancestor tree of w blind to some residue class modulo some prime p? That would be a local obstruction, and it is the first thing that would have to be ruled out before the conjecture (the ancestor tree of 1 is everything) could be true.

**The theorem [Held].**

> **The Tree Has No Blind Spots.** For every prime p, every w with 3 ∤ w, every residue r mod p and every bound B, some n > B reaches w and has n ≡ r (mod p).
>
> **Full Dials** (p ≥ 5). Inside the two-branch skeleton D(v) of the tree, every good 3-adic ending of every length occurs together with every residue mod p.

**What it is not.** It is not the conjecture. It proves nothing about natural density, nothing about covering all integers, and no size bounds. Reading "every ending with every residue" as "most numbers" is the error to avoid. We return to this in §4.

**Sanity example.** Take w = 1 and p = 7. Pure doubling reaches only the residues 1, 2, 4. The theorem says the tripling-undoing branches fill in the other four. Concretely, 1024604, 1024724, 1024256, 1024544, 1024454, 1024805, 1024841 all reach 1 and cover the seven residues 0 to 6 respectively [Held as instances of the theorem, found by search].

**The hypothesis 3 ∤ w is sharp [Held, elementary].** An odd step lands on 1 mod 3, so a multiple of 3 can only be reached by halving. The ancestors of a multiple of 3 are exactly its doublings.

---

## 2. The proof, condensed

### 2.1 The skeleton

Call a number **good** if it is at least 8 and ends in 02 or 22 in base 3 (equivalently, it is 2 or 8 mod 9). Define two maps on good numbers:

- **R(x)** = 4x if x ≡ 2 mod 9, else 16x. Both are the *fewest doublings that land back on good ground*: from a number ending 02, ×4; from one ending 22, ×16, since ×2, ×4, ×8 all fail.
- **L(x)** undoes a tripling: m = (2x − 1)/3 is an odd integer (when x ≡ 2 mod 3) with 3m + 1 = 2x, and then L(x) = 2^i·m with i the fewest doublings landing on a good residue. The padding depends only on x mod 27, because m's last two digits are decided by x's last three: x mod 27 ∈ {2, 8, 11, 17, 20, 26} gives i = 1, 2, 3, 0, 1, 0.

Example: L(8) = 20, with forward trip 20 → 10 → 5 → 16 → 8.

Every number in the skeleton D(v) (everything obtained from a good seed v by R and L) really reaches v **[Held]**. And every w with 3 ∤ w has a good ancestor v obtained by multiplying by a power of 2 between 2⁶ and 2⁹ **[Held]**. So the theorem reduces to a statement about this thin two-branch tree.

### 2.2 The 3-adic side: a tower of rings with doors

Keep only the last K+1 base-3 digits: a **shadow at depth K**, i.e. x mod 3^(K+1). The good shadows are **stations**; there are 2·3^(K−1) of them.

**The ring.** R acts on stations as an invertible map and is transitive **[Held]**, hence a single cycle of length 2·3^(K−1) **[Derived from Held facts; confirmed by direct computation to K = 6]**. At depth 2 it reads 022 → 202 → 222 → 102 → 122 → 002 → back.

**The gearbox [Held].** Name each station by the exponent E with station = 2·4^E (E ranging over 0, 1, 3, 4, 6, 7, … below 3^K). A ring step adds 1 or 2 to E. The dictionary between winding and digits is lifting-the-exponent: v₃(4^m − 1) = 1 + v₃(m). Equivalently, 4^m ≡ 1 mod 3^(K+1) exactly when 3^K | m. A full lap from station 8 winds by exactly 3^K, which is why a lap is a stretch by 4^(3^K).

**The doors [Held].** L reads one more digit than it keeps. If x is known mod 3^(K+2), then L(x) is known mod 3^(K+1): at least K+1 digits survive. So a left turn is a **door** selected by one digit beyond the station's own. Each station has three doors, and each door opens onto a station of the same depth. The ring at depth K+1 is a triple cover of the ring at depth K, and doors run from the upper floor to the lower one. That stacking is the *tower*.

### 2.3 The mod-p side: an affine cocycle, fibres, holonomy

Put a clock with p positions on every station. Every road carries an affine rule on the clock:

- right edge: t ↦ 4t or 16t;
- left edge: t ↦ (μ/3)·t − μ/6, where μ ∈ {2, 4, 8, 16} is the door's padding factor (2·2^i).

This needs 2, 3, 6 invertible, so p ≥ 5; the primes 2 and 3 are handled separately in §2.6. The rule depends only on the road, never on which number travels it. Check with p = 5: the loop 8 → L → 20 → R → 80 acts as t ↦ 4t + 3, and 8, 35, 62 (all ≡ 8 mod 27) map to residues 0, 3, 1 from 3, 0, 2 respectively.

A **fibre** F_K(c) is the set of residues mod p realised by skeleton members with shadow c at depth K. **Holonomy** Hol(K) is the group of rules obtained by composing the labels of closed walks at station 8, forward or backward. It is a subgroup of the affine group of the clock.

**A structural fact that is easy to miss [Derived].** Every slope in Hol(K) lies in the multiplicative group ⟨2, 3⟩ ⊆ (Z/p)*. Right labels have slope a power of 4; left labels have slope μ/3 with μ a power of 2. So Hol(K) ⊆ (translations) ⋊ ⟨2, 3⟩, a proper subgroup of the full affine group whenever ⟨2, 3⟩ is proper (p = 23 is the first such prime; up to 200 they are 23, 47, 71, 73, 97, 167, 191, 193 [Observed by direct computation]). The proof needs only one translation, so this does not matter for CHAT. It matters a great deal for anything that needs more (§5).

### 2.4 The engine: zoom, twist, commutator

Two elements of Hol(K), then one commutator.

**The twist [Held].** Leave station 8 through its door at 8 mod 27, land at some station, ride the ring back to 8. The label is x ↦ αx + β with α ≠ 0 and β = −4^E·(8/6) ≠ 0. For p = 5 this is the loop above: t ↦ 4t + 3.

**The zoom [Held].** A pure stretch λ ∉ {0, 1}. There are three cases, exhaustive:

1. **The lap is visible.** If 4^(3^K) ≠ 1 mod p, a full lap around the ring is a pure stretch by it. (p = 5, K = 1: 4³ ≡ 4.)
2. **The lap is invisible, and ord(4) = 3^a with a ≥ 2.** The dial cannot feel ring distances divisible by 3^a. Take two doors on the same wall whose endings agree to a+1 digits (8 and 8 + 3^(a+1)); both have the same padding, so their twists cancel, and the ring distance between *their sources* is invisible to the dial. But each door loses a digit, so their *landing* stations agree only to a digits. The ring distance between the landings is not a multiple of 3^a, and the dial feels it. Walk out through one door, back through the other, and what survives is a pure stretch.
   *Example, p = 19, depth 2:* the doors with endings 8 and 35 are the same station; they land on 20 and 92 ≡ 11 mod 27; riding from 20 to 11 winds by 3 and 4³ = 64 ≡ 7 ≠ 1 mod 19, so the loop is t ↦ 11t.
   **This is the conceptual centre of the proof: the digit a door forgets is exactly what makes the dial feel something.**
3. **The single exceptional prime.** If ord(4) = 3 then p | 63, so p = 7. Both ring distances are invisible, but the doors with endings 2 and 11 have different padding (4 versus 16), giving a stretch of 16/4 = 4.

Because the depth at which the ratchet below settles cannot be controlled, all three cases must be handled at *every* depth K, and they are: as K grows, a prime can pass from case 1 to case 2 **[Held]**.

**The commutator [Held].** Undo the twist, undo the zoom, redo the twist, redo the zoom. The stretch α cancels (stretches commute) and what remains is a pure slide by (λ − 1)β ≠ 0. Run on t it reads (t − β)/α → (t − β)/(αλ) → (t − β)/λ + β → t + (λ − 1)β. For p = 5, with twist 4t + 3 and zoom 4t, it gives t ↦ t + 4; for p = 7, a slide by 5.

### 2.5 The ratchet and the unsquint

Now convert one slide into every residue everywhere.

1. **Sizes are constant around a ring [Held].** A right edge acts on fibres injectively (its slope is a unit), so fibre size never decreases along the ring; around a cycle, that forces equality. All stations at one depth have fibres of one size.
2. **Sizes can only shrink with depth [Held].** A deeper shadow is finer, so its fibre sits inside the fibre below it. Sizes are integers ≥ 1 (the seed itself is a witness). So they stop falling: there are K₀ and σ with every fibre at depth ≥ K₀ of size σ. This is a pure existence argument; **K₀ is not controlled**.
3. **After the ratchet, doors cost nothing [Held].** A door's source fibre (upper floor) is contained in its station's fibre (lower floor), and both have size σ, so they are equal. Hence every edge acts as a *bijection* between fibres, and so does every walk, forward or backward. This is the step that the claim "a slide fills the clock" depends on: the slide must permute the fibre, not merely move it.
4. **A slide on a prime clock reaches every hour [Held].** The slide by d ≠ 0 maps F_K(8) onto itself. Starting from any residue and repeatedly adding d, you stay in the fibre, and since p is prime d is invertible, so you reach every residue. So σ = p and every fibre at depth ≥ K₀ is the whole clock.
5. **Unsquint [Held].** A fibre at a shallower depth contains the finer fibre above it, so every fibre at *every* depth is full. That is Full Dials.
6. **Largeness [Held].** Any number with ending c is at least c. Take c = 3^(B+2) − 1, all 2s in base 3, which is good. Every skeleton member with that ending exceeds B.

**The small primes [Held].** For p = 2, 3 the clock arithmetic breaks (6 is zero), but explicit constructions suffice: doublings for even targets, and numbers of the form (2^k·w − 1)/3 with k chosen so that 2^k·w ≡ 1 mod 3 (or mod 9 for p = 3, remainder 0).

### 2.6 Why it works, in two sentences

The 3-adic digits and the mod-p clock are two coordinates of one object, and the roads twist the clock as you move on the digits. The twist is non-commutative enough, thanks to the lost digit, that loops produce a slide, and primality turns one slide into the whole clock.

**Where each hypothesis bites.** *p prime*: a slide must generate the clock. *p ≥ 5 in the machine*: labels must be invertible. *3 ∤ w*: otherwise the tree is a single doubling corridor. *The lost digit*: without it the loop group would be too commutative to produce a slide in case 2. *Stabilisation*: existential, which is the reason there are no effective bounds.

---

## 3. A finite model of the forgetting [Held, as mathematics]

Take the six depth-1 endings 2, 8, 11, 17, 20, 26 and draw an arrow from each to every ending in the class where its door lands (the lost digit unknown). Then A has rows (1,0,1,0,1,0) for the first four endings and (0,1,0,1,0,1) for the last two. It has rank 2, A² has rank 1 with every row (2,1,2,1,2,1), A³ = 3A², the characteristic polynomial is X⁵(X − 3), and the number of closed routes of length n is exactly 3^n. Since every row sums to 3, A/3 is a genuine stochastic matrix that **mixes in exactly two steps**, with stationary weights 2/9 on each of 2, 11, 20 and 1/9 on each of 8, 17, 26.

This is the exact shadow of the heuristic "after two left turns the start is forgotten". It is an illustration and is not used in the proof. The modelling assumption, that the lost digit is free, is the one thing nothing here proves about the real map.

---

## 4. The surrounding landscape

*All literature statements in this section are [Memory]. Check before citing.*

**Local-global.** CHAT is a local statement (no obstruction at 3 and at one prime). The conjecture is global (everything is an ancestor of 1). Local statements alone cannot reach the global one, and that gap is the archimedean place.

**Known results of the neighbouring kind.**
- Almost all numbers have finite stopping time (Terras; Everett), and Tao (2019) shows almost all orbits attain almost bounded values, in the sense of logarithmic density. These are *forward* statements and rest on different randomness (see C3 below).
- The number of n ≤ X that reach 1 is at least a power of X; as far as I recall, Krasikov and Lagarias give an exponent near 0.84.
- Cycle results are archimedean: the known obstacles are bounds on how close 2^a and 3^b can be (transcendence-type input), together with large computer verification of trajectories.

**Structural viewpoints.**
- *Conjugacy to the shift on Z₂* (Bernstein, Lagarias): Collatz as a 2-adic shift. The backward tree is the 3-adic counterpart, and CHAT lives there.
- *The ×2 ×3 philosophy* (Furstenberg): rigidity under joint multiplication by 2 and 3. CHAT looks like a finite-field, 3-adic shadow of such rigidity. This is an analogy and not a theorem.
- *Random walks on affine groups over F_p with slopes in ⟨2, 3⟩*: this puts the quantitative questions in the territory of sum-product estimates and expansion in affine groups (Bourgain–Gamburd line of work). I can't give precise statements.
- *Skew products*: the tower with affine labels is a skew product over a 3-adic automaton with an affine cocycle. CHAT says the cocycle is not cohomologous to a trivial one. Ergodicity is the natural language for the limit.

**What is genuinely new here, as far as I can tell [Belief].** The explicit holonomy mechanism, especially the *blur as engine*. I don't know of the same combination elsewhere, but I haven't searched; a literature check should come before any priority claim.

---

## 5. The frontier

### Tier A: low-hanging fruit

**A1. Left-only equidistribution for typical seeds. Start here.** [Derived]
A chain of k left turns from a seed v depends only on v mod 3^(k+2), and each turn consumes one fresh base-3 digit of v. For seeds up to 3^M the pair (ending mod 27, residue mod p), over the first M steps, is up to a CRT error of about p/3^M exactly a Markov chain on 6p states, and the six-state matrix of §3 is its base. Mixing reduces to the transitivity of the loop group on the clock, which is CHAT's content, and quantitatively to the spectral gap of an explicit 6p × 6p matrix. The payoff would be an *effective* theorem: for all but a small fraction of seeds up to X, the residue after k left turns is within ε of uniform once C·log p ≤ k ≤ (horizon), with explicit rates, where CHAT's stabilisation depth is only existential. **Correction to my earlier wording:** *visiting every residue* is a coupon-collector event and needs about p·log p steps, not O(log p). Equidistribution of the residue at a given step is the fast statement (§9).
*Needs proving:* that left-only loops contain a nonzero translation (I expect yes, unproved; the commutator argument above uses right-edge rides), and irreducibility with aperiodicity of the 6p-state chain.
*Catch:* a seed has only about log₃ v digits, so runs are capped at logarithmic length; past that the dynamics are deterministic. The theorem would be "typical seed, orbit of logarithmic length", not an infinite-orbit statement. For 3-adic seeds the full ergodic theorem is available, but integers are a measure-zero set. [Dragon-lite]
*Side note [Derived, quick arithmetic]:* under the chain's stationary weights a left turn multiplies size by about 2^0.75 ≈ 1.7 on average, while consuming log₂3 ≈ 1.58 bits of the seed's digit supply. That comparison is the first thing to examine when asking how far the horizon can be pushed.

**A2. Pin down Hol(K) exactly.** [Picked: see §9]
§2.3 gives the ceiling: translations extended by ⟨2, 3⟩. Computed for 11 primes, including four proper-subgroup primes (23, 47, 71, 73), Hol(K) *equals* the ceiling at K = 1 and 2. So the right conjecture is **Hol(K) = Z/p ⋊ ⟨2, 3⟩** [Observed]. My earlier "full affine group" claim was an artefact of four small primes where ⟨2, 3⟩ happens to be everything.

**A3. Two primes at once.** [Derived, unchecked]
Work in the product of the two affine groups. Translations live in Z/p × Z/q, cyclic of order pq. A subgroup of a cyclic group of order pq that projects onto both factors is everything. So it suffices to find a *joint* translation with both components nonzero, which needs a joint zoom with slope ≠ 1 in both coordinates at once. Correlated slopes in ⟨2, 3⟩ mod p and mod q are where it could fail.

**A4. Prime powers.** [Belief]
Hol into the affine group of Z/p^k. The translation subgroup must generate Z/p^k, so the commutator's slide (λ − 1)β needs to be a unit, which can fail when λ ≡ 1 mod p. Expect a p-adic filtration argument, medium difficulty.

**A5. Effective size bounds.** [Belief]
The ratchet is why none exist. I suspect a direct construction: use the explicit translation loop's length to produce, for each residue, a skeleton member of explicit size, giving an exponential-in-p upper bound on the smallest ancestor with a given residue. That makes CHAT quotable as a computable statement.

**A6. Housekeeping.** Rename `exact_one_step_mixing` (it is a two-step statement). Add references. Check p = 23 before anyone cites "full group". Keep the Lean-faithful paper and the exposition citing each other.

### Tier B: real mathematics, uncertain payoff

**B1. A general CHAT lemma.** Extract: a finite-state automaton with an affine cocycle over a prime field; when does the loop group contain a translation? Test on 5x+1 and 7x+1, where the mismatch is between 2 and a different prime. If it transfers, the structure is the mismatch and not an accident of 3. If it fails, that is informative too.

**B2. The skew-product reading.** Restate CHAT as "the cocycle is not cohomologous to a trivial one" and look for existing ergodic-theoretic results on skew products with affine-group cocycles.

**B3. Sum-product and expansion.** With Hol equal to translations extended by a multiplicative group generated by 2 and 3, quantitative questions about its action on the clock are in sum-product territory. This is where A1's spectral gap could meet existing technology.

**B4. Counting.** Combine the effective chain with a tree count to get lower bounds on ancestors in each residue class up to X. The skinny skeleton is too thin for this alone; it needs the full tree.

### Tier C: dragons

**C1. The archimedean place.** Profinite density means every finite pattern appears at some size. It says nothing about natural density or about covering all integers. The tree grows more slowly than the integers, and the gap between "dense in the profinite sense" and "everything" is the conjecture itself. More of this machinery will not cross it.

**C2. Cycles and divergence.** [Derived] CHAT is evidence that *no congruence-based argument can rule out a nontrivial cycle*: a cycle through c is a finite set of ancestors of c, and the theorem says c's ancestors have no blind spots. Ruling cycles out is archimedean, uses sizes, and belongs to a different world of tools. Don't spend time here without a reason that avoids this point.

**C3. Backward versus forward randomness.** The backward tree consumes fresh 3-adic digits; the forward map consumes 2-adic ones. These are different sources of randomness, and nothing in CHAT is a statement about forward trajectories.

**C4. Quantifier slippage.** "Every seed", "almost every seed" and "the seed 1" are three different theorems. Each sentence of each draft should say which one it is.

**C5. Reading existence as structure.** The ratchet is a pigeonhole. Before building on it, ask whether the mechanism is visible in the explicit loops. (It is, in the commutator and the blur, which is why those are the parts worth generalising.)

---

## 6. Suggested order of work

1. ~~Compute Hol(K) for p = 23 and others (A2).~~ Done, §9: Hol(K) = Z/p ⋊ ⟨2, 3⟩.
2. ~~Check left-only holonomy per prime.~~ Done to p = 97, §9. What remains is a *uniform proof*.
3. Prove the spectral gap (uniform in p, observed to sit between about 0.5 and 0.7) and write A1 as a theorem, with the horizon stated in the abstract.
4. Attempt the two-prime Goursat-style argument (A3), once A2 says what the slope group is.
5. Run the 5x+1 and 7x+1 transfer test (B1).
6. Only then effective bounds (A5) or counting (B4).

**Do not:** chase natural density of the ancestors of 1 directly; try to exclude cycles by congruences; describe CHAT as "progress on Collatz" without saying which gap it leaves.

## 7. How to talk about the work

The accurate sentence: *the inverse Collatz tree of every w not divisible by 3 has no local obstruction at the 3-adic place together with any single prime, and the proof exhibits an explicit affine holonomy mechanism, in which the digit lost by a left turn is what makes the clock turn, that may transfer to other maps.* It does not say Collatz is true and it does not say the tree is large. The paper should be known for the mechanism (the commutator, the ratchet, the blur as engine), not oversold on the gap it leaves.

## 8. Companion documents

*CHAT: Every Remainder, Everywhere* is the intuitive exposition with worked examples. *Inverse Collatz Trees and Simultaneous Density* is the LaTeX paper, faithful to the Lean development. The Lean files hold the formal statements: the main development and the separate six-state amnesia file.

## 9. Fruit picked: computations done after the first draft of this document

All of these are **[Observed]**: exact computation on specific primes, not proofs.

**9.1 The ceiling is the answer (A2).** For p = 5, 7, 11, 13 (as before) and now also 23, 31, 47, 71, 73, 79, 89, with depths K = 1 and 2, the loop group Hol(K), built from forward and backward closed walks at station 8, is exactly (all translations) ⋊ ⟨2, 3⟩. Its order is p·|⟨2, 3⟩|, for instance 253 = 23·11 at p = 23. The slope subgroup produced by loops equals ⟨2, 3⟩ in every case, including where it is a proper subgroup. Conjecture: **Hol(K) = Z/p ⋊ ⟨2, 3⟩ for all p ≥ 5 and all K.** Nothing above the ceiling is possible, and in every computed case nothing below it occurs.

**9.2 Left turns alone already generate it (A1, finite check).** Restrict to *forward* closed walks using only left edges. The set of labels again equals the full ceiling group, containing all p translations, for every prime from 5 to 97 at depth 1 and 2, and at depth 3 for primes below 41. So a left-only translation exists in every tested case, and the commutator argument's use of right-edge rides is not essential. What is missing is a uniform proof.

**9.3 The six-state model extends to a 6p-state chain with a clean spectrum.** States are (ending mod 27, residue mod p); from (e, t) the chain moves to (f, ll(e)(t)) with f uniform among the three endings in e's target class. For every prime tested (5 to 37, then 47, 71, 97, 131):
- the chain is **primitive** (some power has all entries positive, within at most 9 steps), so it is irreducible and aperiodic;
- its stationary law is **exactly uniform in the residue**, times the weights (2, 1, 2, 1, 2, 1)/9 on the six endings, as in §3;
- the second-largest eigenvalue modulus stays between about **0.51 and 0.70** across the whole range, with no visible decay as p grows (p = 131 gives 0.577).

A gap that does not close as p grows is what an expander-type phenomenon looks like, and it would put the quantitative side squarely in the sum-product territory of B3. I have no proof of this; it is the sharpest open target in the document.

**9.4 The chain predicts real integers, and keeps predicting past the digit horizon.** I took 6,000 random good seeds in [3^22, 3^23), iterated the genuine map L, and compared with a simulation of the chain. For p = 7 and p = 23, the fraction of seeds that have visited every residue by step k matched the chain to within sampling noise (p = 7, step 20: 0.623 real versus 0.628 chain; step 44: 0.941 versus 0.947), and the residue distribution at each step was at the sampling noise floor from uniform. This held out to 44 steps, *twice* the digit horizon of about 22. So the horizon in A1 is a limit of what a proof of this shape can reach, not an observed change in behaviour. [Observed, one choice of horizon]

**9.5 A correction about "coverage".** Coverage of all p residues is a coupon-collector event. With the residue near-uniform at each step, covering needs about p·log p steps. For p = 23 the observed coverage by step 44 was about 3.5%, in line with the chain. An honest effective theorem therefore has two parts: fast equidistribution at a given step (about log p steps to mix), and coverage only when the horizon exceeds roughly p·log p, that is for p up to about log X / log log X.

**What to do with this.** The conjecture in 9.1, the left-only generation in 9.2 and the uniform gap in 9.3 are three clean statements, in increasing order of difficulty, each of which a computer cannot settle but now strongly suggests. If the gap can be proved, the effective A1 theorem follows by standard Markov-chain bounds, and the CRT cutoff gives the stated horizon.

---

*Unverified throughout: the uniform-in-p statements in §9 (observed on finitely many primes, not proved); the Goursat reduction; the growth-rate arithmetic; all literature recalled from memory. Everything tagged [Derived], [Belief] or [Memory] is a lead, not a fact.*
