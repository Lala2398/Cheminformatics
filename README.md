# Computational Chemistry — From Theory to Code

Executable Python notebooks that work through **[*Computational Chemistry* (Oxford Chemistry Primers)](https://global.oup.com/academic/product/computational-chemistry-9780198557401?cc=az&lang=en&)** by **Guy H. Grant** and **W. Graham Richards**, chapter by chapter — turning each section of the book into code that runs, produces numbers, and can be argued with.

---

## Contents

| Notebook | Book sections | Code cells | Figures |
|---|---|---:|---:|
| [Chapter 1 — Introduction to Computational Chemistry](#chapter-1--introduction-to-computational-chemistry) | Ch. 1 | 24 | 9 |
| [Chapter 2.1 — Quantum Mechanics](#chapter-21--quantum-mechanics-2127) | §2.1–2.7 | 13 | 7 |
| [Chapter 2.2 — Quantum Mechanics](#chapter-22--quantum-mechanics-28216) | §2.8–2.16 | 16 | 7 |
| [Chapter 3 — Molecular Mechanics](#chapter-3--molecular-mechanics-3136) | §3.1–3.6 | 21 | 9 |
| [Chapter 4 — Statistical Mechanics](#chapter-4--statistical-mechanics-4146) | §4.1–4.6 | 17 | 9 |
| [Chapter 5 — Modelling Biomolecules](#chapter-5--modelling-biomolecules-5155) | §5.1–5.5 | 18 | 7 |
| [Chapter 6 — Ligand Design](#chapter-6--ligand-design-6166) | §6.1–6.6 | 21 | 3 |

Chapter 2 is split into two notebooks because the book's second chapter is long enough that a single file would be unwieldy: **2.1** covers §2.1–2.7 (from wave functions to self-consistent fields), **2.2** covers §2.8–2.16 (from configuration interaction to static indices).

Every notebook is committed **fully executed**, with all outputs and figures present. You can read the results without running anything.

---

## The chapters

### Chapter 1 — Introduction to Computational Chemistry

The book's opening chapter is a survey: what the discipline covers, what hardware it needs, and where its results can be trusted. The notebook treats that survey as a set of testable claims, and doubles as the Python foundation for everything that follows — floats and formatting, lists and loops, dictionaries, regular expressions, decorators, classes, and vectorisation are all introduced on chemical problems rather than toy ones.

The centrepiece is the book's **"black box" warning**. Two functions are built with identical interfaces and different internals; both look correct on the cases you would naturally test, and the notebook then maps the region where one of them silently fails. The scaling discussion is likewise made concrete: the combinatorics of two-electron integrals are counted, then the real scaling exponent is *measured* with a timer rather than asserted, which is why bigger molecules need bigger machines. Later cells build a torsional scan of butane, take it from a potential energy surface to bulk thermodynamics and a heat capacity from energy fluctuations, run a short molecular dynamics trajectory, and close with a neural network written from scratch in NumPy — then check honestly whether it beats a straight line.

### Chapter 2.1 — Quantum Mechanics (§2.1–2.7)

Everything here is in atomic units, and everything is built up rather than called. The Schrödinger equation is turned into a matrix eigenvalue problem by finite differences and solved for a particle in a box; the observable prescription is then applied numerically to those eigenfunctions. Hydrogenic radial functions are plotted, and the distinction between the radial function and the radial distribution is made visible.

Antisymmetry gets a demonstration rather than a statement: a simple product of spin-orbitals is shown to fail under electron exchange, and beryllium's Slater determinant is expanded into all 24 signed products. The molecular orbital sections build the two-centre overlap integral, solve the LCAO problem as a generalised eigenvalue problem, work the book's formal three-orbital cubic, and reduce the whole apparatus to Hückel theory by replacing every matrix element with a parameter. The chapter ends with a **complete self-consistent field calculation on H₂ written from scratch** — STO-3G integrals, the SCF cycle, then a bond-length scan showing what a converged wave function is actually worth.

### Chapter 2.2 — Quantum Mechanics (§2.8–2.16)

The second half deals with everything the single-determinant picture leaves out. Configuration interaction is computed across an entire potential energy curve, so the growing weight of the doubly excited configuration at long bond lengths can be watched directly. The basis set discussion is quantitative: how well three Gaussians actually imitate an exponential, and what a larger expansion buys against what it costs.

Correlation energy is examined for the book's claim that it is roughly constant — and the answer measured here is **two regimes, not one**, which the text reports honestly rather than smoothing over. Semi-empirical methods follow: what dropping the core saves, and for neglect of differential overlap, how many integrals survive and how large the discarded ones really are. The final sections compute Koopmans' theorem against a direct energy difference, Mulliken populations in three basis sets, the molecular electrostatic potential from a converged density, and static reactivity indices for naphthalene against a transition-state-like index.

### Chapter 3 — Molecular Mechanics (§3.1–3.6)

A force field, built one term at a time and then assembled. Bond stretching compares Morse, harmonic and cubic forms; angle bending shows why strained rings need their own parameters; torsions build one-, two- and threefold terms into the combination that describes butane. Non-bonded terms cover Lennard-Jones against Buckingham — including the **Buckingham catastrophe** at short range — constant against distance-dependent dielectric, and a 10-12 hydrogen bond function against the ordinary 6-12. The pieces are then combined into a complete, if minimal, working force field.

Minimization is treated as its own subject: Newton-Raphson derived and implemented, then the book's warning demonstrated — nothing in the method insists on going *downhill*. Steepest descent and conjugate gradients are raced on an elongated quadratic surface, the cost of the Hessian is measured to show why second-derivative methods stay confined to small molecules, and local versus global minima are made unmistakable. Three parameterization claims from the book are each tested separately. The conformational analysis section ends with a rigid pentane scan compared against a relaxed one, a tree search using the book's own ring-closure criterion, and a dihedral driver built on its offset cosine restraint.

### Chapter 4 — Statistical Mechanics (§4.1–4.6)

The chapter opens on the distinction that governs everything after it: **potential energy is not free energy**, shown on a model with two conformers where the lower-energy one is not the populated one. Solvation is then treated both ways — Poisson's equation solved across a dielectric boundary and checked against the Born expression, and solvent-accessible surface area obtained by numerical sphere sampling — followed by the minimum image convention and a measurement of how much energy a cutoff actually discards.

Monte Carlo begins with why uniform random sampling fails: almost every configuration carries no Boltzmann weight at all. A Metropolis simulation of a Lennard-Jones liquid in reduced units follows, and the radial distribution function is extracted from it. Molecular dynamics compares Euler against velocity Verlet, covers thermostatting, explains why the time step is what it is and what constraints buy, and pulls from a trajectory the things a random walk cannot give — displacement, diffusion, correlation functions. Free energy is done on a system whose exact answer is known, then by windowing (eqn 4.40) and thermodynamic integration (eqn 4.42), closing with a thermodynamic cycle for a relative binding free energy.

### Chapter 5 — Modelling Biomolecules (§5.1–5.5)

Why protein structure is not simply computed: the size of conformational space is calculated first, so the rest of the chapter has a reason to exist. Folding is then approached with the simplest model that works — hydrophobic and polar beads on a cubic lattice — and the lattice's own cost is quantified by fitting an ideal α-helix onto lattices of different spacing. Secondary structure prediction is tested on sequences deliberately constructed so that part of their structure is **non-local**, which is where the local methods are expected to fail.

Homology modelling is built in full: dynamic programming on the book's own alignment example, gap penalties and a comparison matrix derived from residue properties, conservation across a family with a planted active site, periodicity detection in the hydrophobicity pattern (the core of the Benner method), 3D-1D environment profiles, and threading one sequence against several candidate folds. The model itself is then assembled — framework from structurally conserved regions, loops by database search matching the framework rather than just the length, sidechains by simulated annealing over rotamers — and validated with the book's checks applied to both a correct model and a deliberately wrong one. The chapter closes on enzyme catalysis: what is lost when a residue is truncated to its functional group, a QM reaction coordinate inside a classical charge field, and the empirical valence bond method as two resonance forms in one secular determinant.

### Chapter 6 — Ligand Design (§6.1–6.6)

The problem turned round: what should bind to the target, and how much can be inferred when the target's structure is unknown. QSAR is fitted as equation 6.1 and then broken deliberately — the fit is excellent and the compound ranking is wrong, because the activity term contains transport as well as affinity. 3D-QSAR follows the book's own arithmetic: a ten-point cubic grid, two fields, 2000 variables from 20 compounds, ordinary least squares against partial least squares, and a coefficient map that recovers the regions which actually determine activity.

Where no receptor structure exists, the notebook builds the active-analog route: least-squares superposition, electrostatic potential matching on an icosahedron when no atom correspondence exists, lowest-energy conformer against a conformational ensemble, pharmacophore extraction with tolerances, and the active/inactive volume subtraction. Where the receptor *is* known, it builds probe maps for three different probe groups over one cleft, multiple copy minimization, and rigid-body docking by Monte Carlo with a cooling schedule — plus tests of soft potentials and of filtering candidate poses with outside information. Generating new structures covers 3D database searching, twenty-point shape screening with precalculated rotations, *de novo* linker assembly between probe-map anchors, and a genetic algorithm with crossover, mutation and selection. Molecular similarity closes the book: the Carbó index against Hodgkin–Richards, electrostatic potential similarity optimized over conformation, and shape similarity applied to a pair of enantiomers.

---

## How the notebooks are written

A consistent structure runs through all seven, and it is worth knowing before reading one.

**Every code cell is followed by a `What we investigated:` block.** This is not a summary of the code. It states what question the cell was asked, what the numbers actually came back as, and what that does or does not establish. Claims in these blocks are checked against the executed output, not written from expectation.

**The book's argument comes first, the code second.** Each section opens with the reasoning as Grant and Richards present it — paraphrased, not quoted at length — so the code has something specific to test rather than merely illustrate.

**Models are small on purpose.** Nearly everything is built from `numpy` and `scipy` directly: Lennard-Jones and Coulomb terms written out, a Metropolis loop, a Verlet integrator, a Kabsch superposition, an SCF cycle. There is no quantum chemistry package and no docking program. The point is to see the mechanism, so the systems are deliberately small enough that the mechanism is not hidden behind a library call.

**Randomness is seeded.** Monte Carlo runs, docking searches, genetic algorithms and random structure libraries all use explicit seeds, so ranks, energies, RMSDs and cluster counts reproduce exactly between runs and between machines. Wall-clock timings do not — see the note on run time below.

**`Note:` blocks flag the Python.** Where a numerical or programming detail matters — why `lstsq` instead of a matrix inverse, why a Lennard-Jones field is capped, why the Kabsch sign correction cannot be skipped — it is called out separately from the chemistry.

---

## Results that did not match expectation

These are the parts worth reading first. In each case the code was left as it ran and the text explains the discrepancy rather than working around it.

- **Correlation energy (Ch. 2.2).** The book's claim that it stays roughly constant holds in one regime and not in another; the notebook reports both rather than quoting the convenient one.
- **Molecular mechanics (Ch. 3).** A rigid conformational scan and a relaxed one disagree about which conformers exist at all — minimisation changes the map, not just its resolution.
- **Statistical mechanics (Ch. 4).** Free energy perturbation fails on a criterion invisible if you inspect only mean energies. The failure lives in the *overlap of the two distributions*, so a perfectly reasonable-looking mean gap can still give a meaningless answer.
- **Modelling biomolecules (Ch. 5).** An enzyme reaction profile computed *in vacuo* does not merely give a poor barrier — the entire catalytic effect lives in the term that is missing, so the calculation cannot describe catalysis at all.
- **Ligand design (Ch. 6), soft potentials.** The stated rationale is that softening the repulsive wall accounts for induced fit. With a rigid receptor it made pose recovery markedly *worse* (8 of 30 versus 26 of 30), because the steep wall was the strongest signal the minimiser had. Softening helps only when something can subsequently relax.
- **Ligand design (Ch. 6), genetic algorithms.** Selection "almost entirely by chance" does not improve slowly — it does not improve at all. The best individual ends below where generation zero started, because without fitness-weighted selection crossover and mutation destroy discoveries as fast as they are made.

---

## Running the notebooks

Python 3.12, with a small and entirely standard stack:

```bash
python -m pip install numpy scipy pandas matplotlib scikit-learn jupyter
```

Then:

```bash
git clone https://github.com/Lala2398/Cheminformatics.git
cd Cheminformatics
jupyter lab
```

Each notebook is self-contained — no data files, no downloads, no external services. Open one and choose **Restart & Run All** to reproduce it from scratch.

### A note on run time

Some cells run a search rather than evaluate a formula: the docking runs and multiple-copy minimizations of Chapter 6, the atom-by-atom shape fitting, the molecular dynamics trajectories of Chapter 4. All of it is plain CPU work, single-threaded in places, so a full re-run takes a few minutes on an unloaded machine and considerably longer on a laptop that is throttling, on battery, or busy with other applications.

Because of this, the **printed timings and speed ratios will differ from run to run** — sometimes by a factor of two or three. Where the text quotes a performance difference it is quoted as an order of magnitude, never as a figure. Everything scientific is seeded and should reproduce exactly; if a rank, energy or cluster count differs on your machine, that is worth reporting.

---

## Scope, and what this is not

This is a **learning and teaching resource**, not a production toolkit.

- The systems are small model systems. Nothing here is validated for research use, and no result should be cited as a calculation on a real molecule.
- The implementations favour transparency over efficiency and over completeness. A textbook force field, a minimal basis SCF and a rigid-body docking score are all written to be read.
- Where the book describes a method that cannot be honestly reproduced at this scale, the notebook says so rather than substituting something that merely looks similar.

If you want to do real work on these problems, the notebooks point toward the established packages rather than competing with them.

---

## Source and attribution

> **Computational Chemistry** (Oxford Chemistry Primers)
> Guy H. Grant and W. Graham Richards
> Oxford University Press — [publisher's page](https://global.oup.com/academic/product/computational-chemistry-9780198557401?cc=az&lang=en&)

The chapter structure, the arguments tested, and the equation numbering follow Grant and Richards. The book's text is **paraphrased throughout**; it is not reproduced. Anyone working through these notebooks should read the primer alongside them — the code tests the book's reasoning and is not a substitute for it.

---

## Author of repository

**Lala Ibadullayeva** — PhD in Computational Structural Biology.
GitHub: [@Lala2398](https://github.com/Lala2398)

Corrections and disagreements are welcome, particularly on the results listed above. If you re-run a notebook and a seeded number comes out differently, please open an issue with your Python and library versions.
