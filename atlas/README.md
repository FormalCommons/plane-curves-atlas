# Plane algebraic curves

This atlas maps the classical theory of plane algebraic curves, as William
Fulton's *Algebraic Curves* presents it [@Fulton2008]: multiplicities and
intersection numbers, Bézout's and Noether's theorems and their applications
to cubics, the resolution of singularities, and the Riemann–Roch theorem
through adjoint curves. Its purpose is to direct a Lean 4 library of this
theory, built on [Tau Ceti](https://github.com/TauCetiProject/TauCeti) and
[Mathlib](https://github.com/leanprover-community/mathlib4). It states the
**milestones**, the results the library must prove, with their sources, what
matters for formalizing them, and what the two libraries already have.

AI agents wrote the pages from the book, and other agents then checked every
statement, location, star and library claim against the printed pages and the
libraries' sources, correcting what was wrong. No person has yet checked the
mathematics. Each atlas page follows one chapter of the book (Chapters 3, 5, 7
and 8), and each auxiliary page one chapter (Chapters 4 and 6) or two
(Chapters 1–2), in the book's order; every page lists every definition,
numbered result and starred problem of its chapters with its status.

## Pages

| Page | Book | Milestones |
|---|---|---|
| [Local properties of plane curves](01-local-properties.md) | Chapter 3 | 16 |
| [Projective plane curves](02-projective-plane-curves.md) | Chapter 5 | 24 |
| [Resolution of singularities](03-resolution-of-singularities.md) | Chapter 7, Appendix A | 16 |
| [The Riemann–Roch theorem](04-riemann-roch.md) | Chapter 8, Appendix A | 14 |
| *Auxiliary:* [Affine algebraic sets and varieties](../auxiliary/affine-varieties.md) | Chapters 1–2 | 10 |
| *Auxiliary:* [Projective varieties](../auxiliary/projective-varieties.md) | Chapter 4 | 8 |
| *Auxiliary:* [Varieties, morphisms and rational maps](../auxiliary/varieties-and-rational-maps.md) | Chapter 6 | 8 |
| **Total** | | **96** |

The [auxiliary pages](../auxiliary/README.md) hold prerequisites that stand on
their own. Their milestones count like the others, but the library should take
from Mathlib and Tau Ceti whatever they already have.

**In scope:** everything the book proves about plane curves, including the
starred problems, whose results the book "used freely in the text"
(preface, 1969). **Out of scope:** general varieties beyond what plane curves
need, higher-dimensional intersection theory, and results that the pages
list as deferred.

## Conventions

- **The field.** $k$ is algebraically closed from Chapter 2 on (the preface
  says from Chapter 3; p. 17 already fixes it). The book assumes
  characteristic zero "whenever convenient", often silently in the problems,
  and Appendix A discusses arbitrary characteristic. Each milestone states
  its characteristic hypothesis exactly; most results hold in every
  characteristic, and the pages say where they do not.
- **Curves are polynomials, not point sets.** An affine plane curve is a
  nonconstant $F\in k[X,Y]$ up to a nonzero scalar, a projective one a
  nonconstant form in $k[X,Y,Z]$ up to a nonzero scalar. Multiple components
  are allowed: $F=\prod F_i^{e_i}$ is not the same curve as $\prod F_i$.
- **Intersection numbers** take values in $\mathbb N\cup\{\infty\}$, with
  $\infty$ exactly when the curves share a component through the point.
- **Orders.** One convention throughout: the order at a place $Q$ of a
  function field $K$ (a point of the nonsingular model, Tau Ceti's
  `TauCeti.Place k K`) is $\operatorname{ord}_Q : K\to$ `WithTop ℤ` with
  $\operatorname{ord}_Q(0)=\top$, Fulton's $\infty$, and
  $\operatorname{ord}_Q(z)=$ `TauCeti.Place.ord Q z` for $z\ne0$: a thin
  wrapper, because Tau Ceti's `Place.ord` has the junk value $0$ at $0$. The
  order of a form $G$ at $Q$ is $\operatorname{ord}_Q(G/L^{\deg G})$ for a linear
  form $L$ not vanishing at the centre of $Q$. Chapter 3's order
  $\operatorname{ord}^F_P$ at a simple point $P$ is Mathlib's `Ring.ord` on the
  discrete valuation ring $\mathcal O_P(\mathbb A^2)/(F)$ (in `ℕ∞`, also $\top$
  at $0$); part (b) of the milestone "Points of a nonsingular projective curve
  are the places of its function field"
  ([resolution page](03-resolution-of-singularities.md#71-rational-maps-of-curves))
  shows that it is the order at the unique place centred at $P$.
- **Numbering.** In this edition propositions and theorems are numbered
  through each chapter, and problems as `n.m` within chapter `n`. The pages
  cite them as "§5.6, Proposition 2" to locate them, and give pages.
- **Library status** on every page was checked on 2026-11-18 against Tau Ceti
  `main` at commit `cd41bff` and Mathlib at `048fffa`, the commit that Tau
  Ceti pins. Both change quickly: re-check before relying on an entry.

## Building the library

The pages give detailed advice for each chapter. Taken together:

**Two layers, and Tau Ceti for the second.** Most of the book is a
*polynomial layer*: curves as polynomials, multiplicities, intersection
numbers, linear systems, Bézout, Noether, cubics, blowing up, adjoints. Almost
none of it exists in either library, and it is the heart of the work. The rest
is a *function-field layer*: places, divisors, the spaces $L(D)$, differentials,
the genus and the Riemann–Roch theorem. Tau Ceti already has that layer, for
function fields over any field, and the library should use it rather than
rebuild it. The book's nonsingular model of a curve is then the set of places
of its function field (Tau Ceti's `TauCeti.Place`), and the library's own work
in Chapters 7 and 8 is to connect the polynomial layer to it: points of a
curve to places, divisors of forms, adjoints and the genus formula.

**Representations the pages recommend.**

- Affine curves in `MvPolynomial (Fin 2) k`, projective ones as homogeneous
  elements of `MvPolynomial (Fin 3) k`; invariants defined on polynomials and
  proved invariant under nonzero scalars. Points of $\mathbb A^2$ as
  `Fin 2 → k`, points of $\mathbb P^2$ as Mathlib's `ℙ k (Fin 3 → k)`.
- The multiplicity $m_P(F)$ is Tau Ceti's `MvPolynomial.orderAt`, which needs
  no chart.
- The local ring $\mathcal O_P(\mathbb A^2)$ as `Localization.AtPrime` of the
  vanishing ideal of $P$, and $\mathcal O_P(\mathbb P^2)$ as
  `HomogeneousLocalization.AtPrime` of `MvPolynomial (Fin 3) k` at the
  homogeneous prime of $P$, with no chart chosen. The intersection number
  $I(P,F\cap G)$ as the `Module.length` of $\mathcal O_P(\mathbb A^2)/(F,G)$,
  and in $\mathbb P^2$ of $\mathcal O_P(\mathbb P^2)/(F_*,G_*)$, in `ℕ∞`, building on the lemmas of Tau Ceti's
  `RingTheory/Intersection.lean`. Never `Module.finrank`, which is $0$ on an
  infinite-dimensional space.
- **Function field and local rings of an irreducible projective plane curve**
  $C=V(F)$: one representation, Fulton's own (§4.2, used in §5.1). $k(C)$ is
  the field of quotients $f/g$ of forms of equal degree in
  $\Gamma_h(F)=k[X,Y,Z]/(F)$, $g\ne0$: Mathlib's
  `HomogeneousLocalization.AtPrime 𝒜 ⊥` for the grading `𝒜` of
  $\Gamma_h(F)$ (Tau Ceti's `HomogeneousIdeal.quotientGrading`), to be shown a
  field. $\mathcal O_P(C)$ is the subring of the $f/g$ with $g(P)\ne0$, a
  `LocalSubring` of $k(C)$, for $P\in C$: the image of
  `HomogeneousLocalization.AtPrime 𝒜 𝔭` ($\mathfrak p=I(P)/(F)$, the
  homogeneous prime of $P$ in $\Gamma_h(F)$, which exists only when
  $P\in C$) under Mathlib's
  `HomogeneousLocalization.mapId`. Why this one: it is the book's definition;
  it needs no chart (no single chart serves every curve: the chart
  $X_i\ne0$ misses the curve $X_i=0$); all local
  rings lie in one field, which is what domination, rational maps and places
  in Chapters 6–8 need (Mathlib's `LocalSubring` order, Tau Ceti's
  `TauCeti.Place k K`); and `AtPrime` is the scheme stalk
  (`AlgebraicGeometry.Proj.stalkIso`). The other descriptions are
  isomorphisms to it, each a milestone or a lemma:
  $\mathcal O_P(C)\cong\mathcal O_P(\mathbb P^2)/(F_*)$, and, in any affine chart
  containing $P$, $k(C)\cong\operatorname{Frac}(k[x,y]/(F_*))$ and
  $\mathcal O_P(C)\cong\mathcal O_P(\mathbb A^2)/(F_*)$ (milestone "Local ring of
  an irreducible projective plane curve as a quotient and in affine charts",
  [Chapter 5 page](02-projective-plane-curves.md#51-definitions)); the scheme
  function field of Tau Ceti's `projectivePlaneCurve` through its
  `functionFieldEquiv` with a chart. Tau Ceti's function-field lemmas on plane
  curves, stated in a chart, transfer along these. For reducible or non-reduced
  $F$ there is no $k(C)$: Chapter 5 works in $\mathcal O_P(\mathbb P^2)/(F_*)$
  and Chapter 3 in $\mathcal O_P(\mathbb A^2)/(F)$, which a chart identifies.
- "No common component" as `IsRelPrime`, not `IsCoprime`.
- Linear systems of curves as subspaces of the forms of degree $n$
  (`MvPolynomial.homogeneousSubmodule`), counted with `finrank`; intersection
  cycles as finitely supported functions.
- Homogenization and dehomogenization in any variable, to a prescribed
  degree, as a central API: Tau Ceti has pieces (`TauCeti.dehomogenize`,
  `MvPolynomial.blockHomogenize`, `Ideal.homogenization`) but not their
  relations.
- No general theory of varieties and no Zariski topology: state
  irreducibility algebraically (prime vanishing ideals, components from
  minimal primes), and on a curve read "open" as "cofinite". Work inside
  function fields: points of a curve give local subrings of its function
  field, and a dominating rational map of curves is a map of function fields.
- The group law on a nonsingular cubic proved as the book does, for any base
  point and characteristic. For a cubic with a flex it is then identified with
  Mathlib's group of a Weierstrass curve by an isomorphism, not a second group
  law there; when $\operatorname{char}k\ne2$ every nonsingular cubic has a flex
  (the Hessian milestone), so this covers all of them. In characteristic $2$
  no milestone yet gives a flex: the identification for arbitrary nonsingular
  cubics there is an open extension.

**Bridges to existing theory.** Several milestones are best proved by
connecting to what exists: the classical local ring of a projective curve
with the stalk of a `Proj` scheme, Tau Ceti's scheme `projectivePlaneCurve`
and its genus with the book's genus formula, and the divisor of a Kähler
differential with Tau Ceti's divisor of a Weil differential. The pages name
the declarations involved.

**Order of work.** The milestones depend on one another in five stages;
within a stage the parts are largely independent, and each stage needs the
ones before it, on the routes the pages recommend; some results move to a
later stage on the book's own routes (for example Theorem 3 and Lemma 2 of
§7.5 through Theorem 2 of §7.4), and the pages say where.

1. *Core.* The milestones of the [affine page](../auxiliary/affine-varieties.md)
   (§§1.6, 1.7, 2.4, 2.6, 2.8, 2.9, 2.10; §2.9 needs §1.7) and all of
   Chapter 3 ([local properties](01-local-properties.md)). Beside Chapter 3,
   needing only the auxiliary pages before them: the milestones of the
   [projective page](../auxiliary/projective-varieties.md) (§§4.1–4.3) and of
   the page on [varieties and rational maps](../auxiliary/varieties-and-rational-maps.md)
   (Chapter 6).
2. §§5.1–5.4 (multiplicities, local rings and intersection numbers in
   $\mathbb P^2$, linear systems, Bézout, multiple points), alongside the
   places of Chapter 7: the milestones of §7.1, and those of §7.5 on the
   nonsingular model, the finiteness of the normalization (Lemma 2) and
   Proposition 2. These use Chapter 6. Within this stage, §5.1 comes first
   for two of them: part (b) of the points-and-places milestone of §7.1 and
   Proposition 2 in its plane form use the local ring and the intersection
   number of §5.1.
3. §§5.5–5.6 (Noether's theorem and its applications, the group law)
   alongside §§7.2–7.4 (blowing up, quadratic transformations, terrible
   points, Noether's formula).
4. Lemma 3 and Proposition 3 of §7.5 (Noether's conditions at an ordinary
   multiple point), which need §5.5 and the orders at the places over an
   ordinary multiple point (§7.2).
5. Chapter 8. Two of its milestones can start earlier: the bridge for
   differentials (§8.4) needs only Tau Ceti, and the divisors cut out by
   forms (§8.1) need only stage 2.

**Already done in the libraries.** These milestones are **full** in Tau Ceti
and should not be counted as work; the library only connects its own
definitions to them:

- [Nonsingular projective plane curves are irreducible](02-projective-plane-curves.md#53-bézouts-theorem)
  (Problem 5.21; `TauCeti.irreducible_of_isHomogeneous`);
- [Nonsingular projective curves correspond to function fields of one variable](03-resolution-of-singularities.md#75-nonsingular-models-of-curves)
  (scheme form);
- [Riemann's theorem on the nonsingular model of a plane curve](04-riemann-roch.md#83-riemanns-theorem)
  (function-field form; its Corollaries 1 and 2 are a few lines each).

Many other milestones are full in part; their "Lean status" says which part,
and the correspondence tables list the definitions and results that Mathlib
or Tau Ceti already has in full, which are not milestones.

**Traps that the pages describe:** Tau Ceti's `Place.ord` is $0$ at $0$ where
the book has $\infty$ (hence the wrapper of Conventions); in Mathlib's order on `LocalSubring`, `A ≤ B` means
that $B$ dominates $A$; `Algebra.trdeg` is a cardinal; Tau Ceti's
`Scheme.IsCurveOver` asks for properness, so it excludes affine curves;
Fulton counts $R$ as a prime ideal and $\emptyset$ as irreducible, Mathlib
does neither.

## Errors in the book

Each page lists the errors and gaps its authors found, in its notes on the
source. Some matter for the statements: the proof that eight points
determine the ninth needs its auxiliary line to avoid the cubic's singular
point; the example of a genus-2 curve on p. 104, $Y^2XZ=X^4+Y^4$, has a
non-ordinary triple point and is rational ($Y^2XZ=X^4+Z^4$, for
$\operatorname{char}k\ne2$, has a single node and genus 2); and several
problems fail in characteristic 2 or 3. The milestones state the corrected
results, and say where they go beyond the book.

The pages will still contain errors. A library built from this atlas should
fix those it finds as part of the work and record them; the library's
`ERRATA.md` collects them.
