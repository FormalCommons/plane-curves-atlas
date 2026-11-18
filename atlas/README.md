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
mathematics. Each page follows one chapter of the book, in its order, and
lists every definition, numbered result and starred problem of the chapter
with its status.

## Pages

| Page | Book | Milestones |
|---|---|---|
| [Local properties of plane curves](01-local-properties.md) | Chapter 3 | 16 |
| [Projective plane curves](02-projective-plane-curves.md) | Chapter 5 | 25 |
| [Resolution of singularities](03-resolution-of-singularities.md) | Chapter 7 | 15 |
| [The Riemann–Roch theorem](04-riemann-roch.md) | Chapter 8, Appendix A | 14 |
| *Auxiliary:* [Affine algebraic sets and varieties](../auxiliary/affine-varieties.md) | Chapters 1–2 | 12 |
| *Auxiliary:* [Projective varieties](../auxiliary/projective-varieties.md) | Chapter 4 | 10 |
| *Auxiliary:* [Varieties, morphisms and rational maps](../auxiliary/varieties-and-rational-maps.md) | Chapter 6 | 8 |

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
- **Numbering.** In this edition propositions and theorems are numbered
  through each chapter, and problems as `n.m` within chapter `n`. The pages
  cite them as "§5.5, Proposition 2" to locate them, and give pages.
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
  vanishing ideal of $P$, and the projective one as
  `HomogeneousLocalization.AtPrime`. The intersection number
  $I(P,F\cap G)$ as the `Module.length` of $\mathcal O_P(\mathbb A^2)/(F,G)$,
  in `ℕ∞`, building on the lemmas of Tau Ceti's
  `RingTheory/Intersection.lean`. Never `Module.finrank`, which is $0$ on an
  infinite-dimensional space.
- "No common component" as `IsRelPrime`, not `IsCoprime`.
- Linear systems of curves as subspaces of the forms of degree $n$
  (`MvPolynomial.homogeneousSubmodule`), counted with `finrank`; intersection
  cycles as finitely supported functions.
- Homogenization and dehomogenization in any variable, to a prescribed
  degree, as a central API: Tau Ceti has pieces (`TauCeti.dehomogenize`,
  `MvPolynomial.blockHomogenize`, `Ideal.homogenization`) but not their
  relations.
- No general theory of varieties or of the Zariski topology beyond a thin
  layer that the pages name. Work inside function fields: points of a curve
  give local subrings of its function field, and a dominating rational map
  of curves is a map of function fields.
- The group law on a nonsingular cubic proved as the book does, for any base
  point and characteristic, and then identified with Mathlib's group of a
  Weierstrass curve by an isomorphism, not a second group law there.

**Bridges to existing theory.** Several milestones are best proved by
connecting to what exists: the classical local ring of a projective curve
with the stalk of a `Proj` scheme, Tau Ceti's scheme `projectivePlaneCurve`
and its genus with the book's genus formula, and the divisor of a Kähler
differential with Tau Ceti's divisor of a Weil differential. The pages name
the declarations involved.

**What can proceed in parallel.** After a short core (polynomials and
points, homogenization, multiplicity, the local ring at a point, and the
intersection number with its properties), the work splits into largely
independent branches:

- projective plane curves: linear systems, Bézout, multiple points;
- Noether's fundamental theorem and its applications: eight points
  determine the ninth, Pascal and Pappus, the group law on a cubic, flexes;
- blowing up points and quadratic transformations;
- function fields, places and the nonsingular model;
- adjoints, the genus formula and canonical divisors, which join the last
  three branches at the end;
- the auxiliary results that the libraries lack.

**Traps that the pages describe:** Tau Ceti's `Place.ord` is $0$ at $0$ where
the book has $\infty$; in Mathlib's order on `LocalSubring`, `A ≤ B` means
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
