# Experimental Checks for the CHAT Papers: A Report (Revised)

*Auxiliary paper, second version. This is a complete account of the computations run in support of the CHAT papers: what each was designed to test, how, what came out, and what it does not show. Nothing here is a proof. Every claim is tagged **\[reproduced\]** (recomputed from scratch for this report), **\[extended\]** (a claim from the working notes tested further), **\[not reproduced\]** (a claim from the notes that we did not re-run), or **\[derived\]** (derived by hand and then machine-checked).*

## 1. Scope, conventions, reproducibility

Definitions are those of Paper One: $m(x)$ by $x\\bmod27$ ($m=4,8,16,2,4,2$ at $2,8,11,17,20,26$), $L(x)=m(x)(2x-1)/6$, $R(x)=4x$ if $x\\equiv2\\pmod9$ and $16x$ otherwise, the **tower graph** $\\Gamma_K$ (vertices: valid classes modulo $3^{K+1}$; right edges $c\\to R(c)$; left edges $e\\bmod3^{K+1}\\to L(e)\\bmod3^{K+1}$ indexed by valid $e\<3^{K+2}$), and the **cell graph** (six classes modulo $27$; a cell leads to the three cells of its image class modulo $9$).

Affine maps are pairs $(a,b)$ for $t\\mapsto at+b$, composed *first $f$, then $g$*: $f\\cdot g=(g_1f_1,\\ g_1f_2+g_2)$. The commutator is $f^{-1}g^{-1}fg$ in this product. All code is Python 3 (standard library, `fractions`, `numpy`, `sympy` for orders and factorisation). Exact integer or `Fraction` arithmetic throughout, except for the growth rate in E2, which uses floating-point logarithms of exact integers. The sampling experiments (E2 growth and cell frequencies) use unseeded random starts, so their last digits vary between runs.

## 2. Summary of experiments

| ID | Question | Outcome | Status |
| --- | --- | --- | --- |
| E1 | Loop labels and commutators of $W_1,W_4,W_5$ | $5/9$, $19/27$ exactly | reproduced |
| E2 | Structural battery on $L$ (cell-to-class map, expansion, valuation law, measure, growth) | all pass | reproduced |
| E3 | $R$-cycle length on $U_K$ | single cycle, length $2\\cdot3^{K-1}$, $K\\le6$ | reproduced |
| E4 | Closed words and integer cycles of $L$ | $3^\\ell$ words; only cycle is ${2}$; $\\ell\\le8$ | reproduced, extended |
| E5 | Number theory of $\\langle2,3\\rangle$ and Zsigmondy primes | criteria confirmed; $307$; list corrected | reproduced |
| E6 | Cell-graph holonomy order | $=p\\cdot\\lvert\\langle2,3\\rangle\\rvert$, $5\\le p\\le200$ | reproduced, extended |
| E7 | Tower holonomy modulo $N$ | full translations $\\rtimes\\langle2,3\\rangle$ | reproduced, extended |
| E8 | Do the loops lift to closed walks in $\\Gamma_K$, and where? | yes, at distinct vertices from level $3$ on | derived, machine-checked |
| E9 | Are tower commutator translations nonzero? | never both pairs zero, $p\<400$, $K\\le7$ | reproduced |
| E10 | Coverage by breadth-first search in $D(8)$ | all pairs by depth $6$–$7$, $p\\in{5,7,11,13}$ | reproduced |
| E11 | Covering time of the pure left orbit | no failures below $6000$; median $\\approx9p\\ln(6p)$-order | reproduced (small range) |

## 3. Experiments

**E1. Loops and commutators.** *Method:* compose the left labels $(m/3,-m/6)$ along the multiplier sequences $\[4\]$, $\[4,4,2,2\]$ and $\[4,4,2,2,2\]$; form $\[W_1,W_4\]$ and $\[W_1,W_5\]$. *Result:* $W_1=(\\tfrac43,-\\tfrac23)$, $W_4=(\\tfrac{64}{81},-\\tfrac{101}{81})$, $W_5=(\\tfrac{128}{243},-\\tfrac{283}{243})$, with fixed points $2$, $-\\tfrac{101}{17}$, $-\\tfrac{283}{115}$ and fixed-point differences $\\tfrac{135}{17}$, $\\tfrac{513}{115}$. The commutators are the pure translations $5/9$ and $19/27$, so $-5\\cdot\\tfrac59+4\\cdot\\tfrac{19}{27}=\\tfrac1{27}$. The slope exponent vectors $(2,-1),(6,-4),(7,-5)$ have pairwise determinants $-2,-3,-2$.

**E2. Structural battery on $L$.** Each item is a single short script.

- *Cell-to-class map.* For every admissible $x\<3^7$, $L(x)\\bmod9$ is $2$ when $x\\bmod27\\in{2,8,11,17}$ and $8$ when it is in ${20,26}$. No exceptions.
- *Exact expansion.* For $20{,}000$ random pairs $x,y=x+27\\cdot3^k t$ in the same cell, $v_3(L(y)-L(x))=v_3(y-x)-1$ with no violations.
- *Valuation law.* Over $400$ random $300$-digit ternary starts and $150$ steps each ($60{,}000$ steps), $v_2(L(x))=\\log_2m(x)-1$ always.
- *Invariant measure.* Cell frequencies along these orbits: $0.224,0.109,0.224,0.113,0.221,0.109$ for cells $2,8,11,17,20,26$, against the predicted $2/9,1/9,2/9,1/9,2/9,1/9$.
- *Growth.* Mean of $\\bigl(\\ln L^{150}(v)-\\ln v\\bigr)/150$ is $0.5197$ against the predicted $\\tfrac73\\ln2-\\ln3=0.5187$.

*Meaning.* The statements of §10 on the left branch survive an independent test, for typical large starts. Note that these starts are large integers standing in for $3$-adic points, so this is evidence for the measure-theoretic claims, not a proof.

**E3. The $R$-cycle.** Iterating $c\\mapsto R(c)\\bmod3^{K+1}$ from $8$ returns to $8$ after exactly $2\\cdot3^{K-1}$ steps for $K=1,\\dots,6$ (lengths $2,6,18,54,162,486$), the number of valid classes. This is the single-cycle statement of Lemma 3.

**E4. Closed words and integer cycles.** Cyclic cell words of length $\\ell\\le8$ were enumerated in the cell graph, giving $3,9,27,81,243,729,2187,6561=3^\\ell$ words. The adjacency matrix has $\\operatorname{Tr}A^n=3^n$ for $n=1,\\dots,8$ and eigenvalues $3$ and five zeros. For each word the composite affine map was solved for its fixed point, kept if a positive integer, and verified by iterating $L$ along the prescribed cells. *Result:* the only positive integer periodic point is $2$. This extends the notes' $\\ell\\le5$ to $\\ell\\le8$ and adds no information on the conjecture.

**E5. Number theory.**

- The multiplicative order of $4$ is $3$ at $7$, $9$ at $19$ and $73$, and $81$ at $163$ and at $2593$. At $717121$ it is $264$, not a power of $3$. Since $\\Phi\_{27}(4)=3\\cdot262657\\cdot87211$, the primes of order $27$ are $262657$ and $87211$. This corrects a list in a draft that placed $2593$ and $717121$ at order $27$.
- For all primes $5\\le p\<6000$: $\[\\mathbb F_p^\\times:\\langle2,3\\rangle\]$ is even iff $p\\equiv\\pm1\\pmod{24}$, and divisible by $3$ iff $p\\equiv1\\pmod3$ and both $2$ and $3$ are cubes. The first primes with index divisible by $3$ are $307,439,499,643,727,919$. The candidate $223$ fails ($3^{74}\\equiv183$). Also $3^{102}\\equiv2^{102}\\equiv1\\pmod{307}$. The first primes with even index are $23,47,71,73,97,167$.

**E6. Cell-graph holonomy.** *Method:* breadth-first search over states (cell, affine map mod $p$) starting from $(2,\\mathrm{id})$, applying forward edges, and collecting the maps at cell $2$ (a finite subsemigroup of $\\mathrm{Aff}(\\mathbb F_p)$, hence a group). *Result:* its order equals $p\\cdot|\\langle2,3\\rangle|$ for all $44$ primes $5\\le p\\le200$. (The notes claim $p$ up to about $260$; we confirmed $200$.)

**E7. Tower holonomy modulo $N$.** *Method:* build $\\Gamma_K$ with labels modulo $N$; take a spanning tree of the underlying undirected graph rooted at $8$; each non-tree edge gives a loop at $8$; close the loop labels under product by breadth-first search in $\\mathrm{Aff}(\\mathbb Z/N)$; record the group order, the number of slope-one elements, and $|\\langle2,3\\rangle|$. *Results:* for $K=1,2,3$ and $N\\in{5,7,11,13,17,19,23,25,29,31,35,49,55,77,125}$, for $N=307$ with $K=1,2$, and for $K=4$ with $N\\in{5,7,13,23,35}$, the slope-one elements are exactly the $N$ translations and the slope group is exactly $\\langle2,3\\rangle$, so $|\\mathrm{Hol}|=N\\cdot|\\langle2,3\\rangle|$ every time. Selected values:

| $N$ | slope group | order of Hol |
| --- | --- | --- |
| $5$ | $4$ | $20$ |
| $7$ | $6$ | $42$ |
| $13$ | $12$ | $156$ |
| $23$ | $11$ | $253$ |
| $35$ | $24$ | $840$ |
| $125$ | $100$ | $12500$ |
| $307$ | $102$ | $31314$ |

The holonomy is already full at $K=1$. *Meaning:* no arithmetic obstruction to a unit translation appeared in the tower, including at the composite and prime-power moduli the case-split proof could not control.

**E8. Do the loops lift to closed walks in $\\Gamma_K$?** *Derivation:* a cell word whose $3$-adic periodic point is $x$ lifts to a closed walk in $\\Gamma_K$ by taking edges $e_i\\equiv L^i(x)\\pmod{3^{K+2}}$, since $L(e_i)\\equiv L^{i+1}(x)\\pmod{3^{K+1}}$. The walk closes at the vertex $x\\bmod3^{K+1}$. *Check:* for each $K=2,\\dots,7$ and each of $W_1,W_4,W_5$ we computed the exact $3$-adic orbit (rational arithmetic), reduced modulo $3^{K+2}$, and asserted that cells, successor vertices and closure hold. All assertions passed. Resulting base vertices:

| $K$ | $W_1$ | $W_4$ | $W_5$ |
| --- | --- | --- | --- |
| $2$ | $2$ | $2$ | $2$ |
| $3$ | $2$ | $56$ | $56$ |
| $4$ | $2$ | $137$ | $137$ |
| $5$ | $2$ | $380$ | $137$ |
| $6$ | $2$ | $380$ | $1595$ |
| $7$ | $2$ | $380$ | $1595$ |

So at level $2$ all three loops close at vertex $2$ with no drift. From level $3$ on they sit at different vertices, as the vertex $x_4\\equiv x_5\\equiv56\\pmod{81}$ differs from $2$; $x_4$ and $x_5$ separate at level $5$. A draft's claim that $x_4\\equiv47$, $x_5\\equiv26\\pmod{81}$ is false.

**E9. Are the tower translations nonzero?** *Method:* for each $K=2,\\dots,7$ and each prime $5\\le p\<400$, conjugate each loop to the base vertex $8$ by the $R$-path found by stepping from $8$ to the loop's vertex, form the commutator in $\\mathrm{Aff}(\\mathbb F_p)$ (asserting that its slope is $1$), and record whether the translation is zero. Pairs tested: $(W_1,W_4)$, $(W_1,W_5)$, $(W_4,W_5)$. *Result:*

| $K$ | $(W_1,W_4)$ zero at | $(W_1,W_5)$ zero at | both zero |
| --- | --- | --- | --- |
| $2$ | $5$ | $19$ | none |
| $3$ | $5$ | $19$ | none |
| $4$ | $5,11$ | $19,59$ | none |
| $5$ | $5,11$ | $19$ | none |
| $6$ | $5,11$ | $19$ | none |
| $7$ | none | $19$ | none |

No prime below $400$ kills both pairs at any tested level, and no prime kills all three pairs. The failures at $11$ and $59$ agree with the derived condition that the $(W_1,W_4)$ translation vanishes when $34\\cdot4^c+101\\equiv0\\pmod p$ with $c$ the exponent difference of the connecting $R$-paths (for $p=11$: $34\\equiv1$, $101\\equiv2$, so $4^c\\equiv9$, true for $c=3$). *Meaning:* the unit property in the tower is not automatic but also does not fail where we looked. This is evidence about the base-vertex gap of §11.3, not a proof for all $K$ and $p$.

**E10. Coverage by breadth-first search in $D(8)$.** *Method:* generate descendants of $8$ generation by generation (deduplicated), record the pairs (class modulo $27$, residue modulo $p$), and stop when all $6p$ pairs are present. *Result:* $p=5$: depth $6$ ($127$ nodes); $p=7,11,13$: depth $7$ ($255$ nodes). This illustrates Theorem 1 and the full fibres at level $2$. A truncated tree can only undercount, so it provides no evidence beyond illustration.

**E11. Covering time of the pure left orbit.** *Method:* for each of the $1332$ admissible starts $\<6000$, follow $L^n(v)$ with exact integers until all pairs (cell mod $27$, residue mod $p$) have been visited, capped at $3000$ steps. *Result:* $p=5$ ($30$ pairs): no failures, median $138$, 95th percentile $252$, maximum $704$. $p=7$ ($42$ pairs): no failures, median $216$, 95th percentile $377$, maximum $699$. A rough estimate $9p\\ln(6p)$ gives $\\approx153$ and $\\approx236$, the right order, with the constant governed by the rarest pair (probability $1/(9p)$).

## 4. What the experiments corrected

1. The commutator in the convention of Paper One is $5/9$ and $19/27$. Working notes with an extra factor $1/(\\alpha\\mu)$ gave $-135/256$ and $-513/512$, and an earlier version of Part III built a false "two conventions" table on this.
2. The drift residues $47$ and $26$ modulo $81$ are false; the correct values are $56$ and $56$.
3. The primes $2593$ and $717121$ are not of order $27$; $163$ is a genuine order-$81$ prime and had been wrongly removed from a list.
4. The prime $223$ is not the first with index divisible by $3$; $307$ is.
5. The closed-word and no-cycle check now covers $\\ell\\le8$, not $5$.
6. The identification of a base-vertex gap needs refinement: at level $2$ there is no drift (E8), so the short proof is complete if $K_0\\le2$.

## 5. Not reproduced, not done

Not reproduced: the notes' sample of about $650$ integers below $10^9$ and its failure rates ($3%,16%,79%$ at $300$ steps); the truncated-tree numerics (pair coverage modulo $25,49,125,343,35,55,77$ and products; fibre fullness for thirteen starts; the $X^{0.88}$ count; depth $8$ for modulus $35$); the holonomy orders for $p\\in(200,260\]$; the length-graded holonomy index at $p=7,17,31,41,79$. Not done: a Lean recompile (the compile status is the file header's); any test of the density-one argument beyond E11; any computation over $\\mathbb Q$ of the translation module (E7 is modulo selected $N$); extension of E9 beyond $K=7$ and $p=400$.

## 6. Limits of the evidence

- The experiments check consistency of claims; they do not prove any. In particular E7 and E9 are finite and do not show the holonomy is full for all $K$ and $N$.
- Several tests reuse the definitions as we wrote them, so a misreading shared by the tests and the text would go undetected. E1, E8 and E9 were derived and tested independently of the earlier drafts; E2 and E4 reproduce statements derived there.
- Sampling results (E2, E11) depend on the sampled starts; E11 used small starts, which do not probe the exceptional integers $2+3^ku$ except at small $k$.
- No experiment here bears on stabilisation ($\\sigma_1=\\sigma_2$), on every-integer covering, or on convergence.

## 7. Code

**Common definitions.**

```python
from fractions import Fraction as F
def mult(x): return {2:4,8:8,11:16,17:2,20:4}.get(x%27,2)
def L(x): return mult(x)*(2*x-1)//6
def R(x): return 4*x if x%9==2 else 16*x
```

**E1.**

```python
def lab(m): return (F(m,3),F(-m,6))
def then(f,g): return (g[0]*f[0], g[0]*f[1]+g[1])
def inv(f): return (1/f[0], -f[1]/f[0])
def comp(ms):
    r=(F(1),F(0))
    for m in ms: r=then(r,lab(m))
    return r
def comm(f,g): return then(then(then(inv(f),inv(g)),f),g)
W1,W4,W5=comp([4]),comp([4,4,2,2]),comp([4,4,2,2,2])
print(comm(W1,W4),comm(W1,W5))   # (1, 5/9) (1, 19/27)
```

**E8 (closure of loops in $\\Gamma_K$).**

```python
def lift(fr,k): return fr.numerator*pow(fr.denominator,-1,3**k)%3**k
def Lq(x):  # exact 3-adic L on a rational point
    m=mult(lift(x,3)); return F(m)*(2*x-1)/6
def check_closed(fix,cells,K):
    x=fix; es=[]
    for c in cells:
        e=lift(x,K+2); assert e%27==c; es.append(e); x=Lq(x)
    M=3**(K+1)
    for i,e in enumerate(es):
        assert L(e)%M==es[(i+1)%len(es)]%M
    return es[0]%M
# fixed points: W1: F(2); W4: F(-101,17); W5: F(-283,115)
# cell words: [2], [2,20,26,17], [2,20,26,26,17]
```

**E9 (tower commutator translations modulo $p$).**

```python
def lab_p(m,p): return (m*pow(3,-1,p)%p,(-m*pow(6,-1,p))%p)
def then_p(f,g,p): return (g[0]*f[0]%p,(g[0]*f[1]+g[1])%p)
def inv_p(f,p):
    a=pow(f[0],-1,p); return (a,(-a*f[1])%p)
def rexp(K,v):          # exponent of the R-path 8 -> v in Gamma_K
    M=3**(K+1); c=8; E=0
    for _ in range(2*3**(K-1)+1):
        if c==v: return E
        E+=1 if c%9==2 else 2; c=R(c)%M
def translation(K,p,loop_labels,vertices):
    g=[]
    for lab_,v in zip(loop_labels,vertices):
        P=(pow(4,rexp(K,v),p),0)
        g.append(then_p(then_p(P,lab_,p),inv_p(P,p),p))
    f,h=g
    c=then_p(then_p(then_p(inv_p(f,p),inv_p(h,p),p),f,p),h,p)
    assert c[0]==1
    return c[1]
```

**E7 (tower holonomy, outline).** Vertices: valid classes mod $3^{K+1}$. Edges: `(c, R(c)%M, 'R')` and `(e%M, L(e)%M, 'L', mult(e))` for valid `e<3^(K+2)`. Labels mod $N$; spanning tree from $8$ by BFS on the undirected graph; loop labels `then(then(pot[u],lab),inv(pot[w]))` for non-tree edges; closure by BFS in $\\mathrm{Aff}(\\mathbb Z/N)$; compare the count of slope-one elements to $N$ and the slope-group order to $|\\langle2,3\\rangle|$.

**E6, E10, E11.** E6 runs a forward BFS over states `(cell, a, b)` from `(2,1,0)` with next cells from the image class; E10 generates descendants by `{R(x), L(x)}` per generation with deduplication; E11 steps `x -> L(x)` with a set of visited `(x%27, x%p)` pairs. Each is a few lines.