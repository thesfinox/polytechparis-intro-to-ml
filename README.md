# Introduction to Machine Learning: Course Slides (2026)

Beamer slide decks for the 2026 edition of the _Introduction to Machine Learning_ course at **Polytech Paris-Saclay** (programme ET4 / ET4APP5, Matériaux). Instructor: Riccardo Finotello (CEA).

The course is a 16-hour practical introduction to machine learning for materials science students (4 sessions x 4h). This repository contains the source of the lecture decks, the custom code-listings package they rely on, and the compiled PDFs.

## Idea Behind the Repository

The course is built around three pedagogical decisions, and the slides implement all three:

1. **Theory left, code right.** Every concept is presented on a slide split into two columns: the conceptual explanation and formulas on the left, a runnable Python snippet on the right. The slide deck is the single source of truth for what is taught; the hands-on Jupyter notebooks (distributed outside this repository) reuse the exact code patterns shown in the slides.
2. **One dataset throughout.** The entire course runs on a single dataset: hyperspectral observations from the Mars Reconnaissance Orbiter (MRO, CRISM instrument). The same data is explored (Module 1), modelled (Module 2), reduced and clustered (Module 3), and finally fed to neural networks (Module 4), as tabular spectra then as image patches. The slides always refer back to this dataset, so students never change domain between modules.
3. **Math with code.** Statistics, optimisation, and backpropagation are shown, explained, and immediately coded. Formulas are displayed and interpreted, but derivations are geometric and visual rather than algebraic, matching the audience baseline (weak formal maths, solid domain knowledge).

The target audience is a dual cohort of étudiants and apprentis with limited Python experience. The slides therefore keep every terminal command copy-paste ready (`uv init`, `uv add`, `uv sync`, `uv run jupyter lab`) and every code snippet self-contained.

## Modules

Each module is one 4-hour session and one deck:

| File | Module | Content | Date |
|:-----|:-------|:--------|:-----|
| `01_intro_ml_polytech.tex` | 1. Foundations: Python, Statistics, and EDA | UV project setup, statistics (mean, variance, covariance, correlation), exploratory data analysis on Mars spectra | 30 November 2026 |
| `02_intro_ml_polytech.tex` | 2. ML Practices, Optimisation, and Linear Models | Train/validation/test splits, feature scaling, scikit-learn Pipelines, Lagrangian intuition for regularisation, Ridge/Lasso, linear and logistic regression | 2 December 2026 |
| `03_intro_ml_polytech.tex` | 3. The Algorithm Toolbox | SVM and the kernel trick, decision trees and ensemble intuition, PCA, K-means and DBSCAN clustering on Mars spectra | 7 December 2026 |
| `04_intro_ml_polytech.tex` | 4. Neural Networks | Perceptron to feed-forward networks, gradient descent and backpropagation via computational graphs, PyTorch code-along, CNN theory and live demo | 9 November 2026 |

Each deck is a thin wrapper: a title page, a placeholder block where content slides are included, and a closing "thank you" page. Content slide fragments live in the corresponding `NN_slides/` directory and are pulled in with `\input` (see [Repository Layout](#repository-layout)). The decks currently carry only the scaffolding; the per-session fragments are being authored into the `NN_slides/` folders.

## Repository Layout

```
slides/
|-- 01_intro_ml_polytech.tex      Module 1 deck (title + includes + last page)
|-- 01_intro_ml_polytech.pdf      Compiled Module 1 (Git LFS)
|-- 02_intro_ml_polytech.tex
|-- 02_intro_ml_polytech.pdf      (Git LFS)
|-- 03_intro_ml_polytech.tex
|-- 03_intro_ml_polytech.pdf      (Git LFS)
|-- 04_intro_ml_polytech.tex
|-- 04_intro_ml_polytech.pdf      (Git LFS)
|-- 01_slides/                    Module 1 slide fragments (one file per section)
|-- 02_slides/                    Module 2 slide fragments
|-- 03_slides/                    Module 3 slide fragments
|-- 04_slides/                    Module 4 slide fragments
|-- beamer-code-listings.sty      Carbon-style code card package (in-repo, not on CTAN)
|-- code_listings_demo.tex        Demo deck exercising every card style
|-- code_listings_demo.pdf        Compiled demo (Git LFS)
|-- img/
|   `-- titlepage.png             Title page picture used by all decks
|-- .gitattributes                Git LFS rules for binary assets
`-- .gitignore                    LaTeX auxiliary files, generated PDFs
```

### Slide fragments

The `NN_slides/` folders hold one file per section (for example `01_slides/section_01.tex`). Fragments contain only `\section{...}` and `\begin{frame}...\end{frame}` environments: no preamble, no `\begin{document}`. They are included from the deck with:

```latex
\input{01_slides/section_01}
\input{01_slides/section_02}
```

## Code Cards: `beamer-code-listings`

The course prints a lot of Python, so the repository ships a small LaTeX package, `beamer-code-listings.sty`, that renders code as Carbon-style "code snapshot" cards. It provides:

- A **dark card** using the Carbon "One Dark" palette (background `#282C34`, keyword `#C678DD`, strings `#98C379`, window dots like the Carbon web tool).
- A **light card** variant (Atom One Light palette) for slides on light backgrounds.
- **Jupyter notebook cells** with `In [n]:` / `Out[n]:` prompts for the code-along segments.
- `tcolorbox`-based cards that break across pages, with line numbers.

The demo deck `code_listings_demo.tex` shows every available style and is the reference for usage. Compile it with the same commands as the module decks.

## Build Requirements

- **TeX Live** with `pdflatex` (the decks use the classic `[t,10pt,aspectratio=169,rounded]` Beamer class).
- The **CEA23 Beamer theme**, installed in the local texmf tree so kpathsea finds it:
  `~/texmf/tex/latex/beamer/themes/beamercea/beamerthemeCEA23.sty`.
  The theme provides the `graphics/` assets used by the title page (`graphics/ORIGINAL/ISAS.png` for the title page logo) and the background, section, and closing-page templates.
- The packages `listings`, `xcolor`, `tcolorbox` (with the `listings`, `skins`, and `breakable` libraries), `csquotes`, and `babel` (English).
- `img/titlepage.png` (in-repo) for the title page picture.

The in-repo `beamer-code-listings.sty` is found automatically because it sits in the working directory. The theme files are **not** vendored in this repository; they come from the installed texmf tree (see [Installing the Theme](#installing-the-theme)).

### Compiling a deck

```bash
pdflatex -interaction=nonstopmode 01_intro_ml_polytech.tex
```

or with `latexmk`:

```bash
latexmk -pdf -interaction=nonstopmode 01_intro_ml_polytech.tex
```

Run the same command for modules 2, 3, and 4, and for `code_listings_demo.tex`.

### Installing the Theme

The theme is distributed from the CEA intra GitLab (`https://www-git-cad.intra.cea.fr/DEC/beamercea.git`). To install it into the user texmf tree:

```bash
git clone --depth 1 https://www-git-cad.intra.cea.fr/DEC/beamercea.git /tmp/beamercea
mkdir -p ~/texmf/tex/latex/beamer/themes
cp -r /tmp/beamercea/beamerthemeCEA23.sty /tmp/beamercea/graphics ~/texmf/tex/latex/beamer/themes/beamercea/
```

## Version Control

- **Git LFS** tracks the binary assets (`*.pdf`, `*.png`, `*.jpg`, `*.jpeg`, `*.tif`, `*.tiff`, `*.gif`, `*.bmp`, `*.webp`, `*.ico`, `*.eps`, `*.eot`, `*.otf`, `*.woff`, `*.woff2`, `*.zip`, `*.gz`, `*.tar`) via `.gitattributes`. The compiled PDFs are stored as LFS objects, so cloning and pulling requires `git lfs pull`.
- **Compiled PDFs are tracked but rebuilds are normal.** Each commit that changes a deck's `.tex` also updates the corresponding `.pdf` so the repository always contains a buildable, up-to-date snapshot of every deck.
- **Conventional Commits** are used: `style(slides): ...`, `feat(listings): ...`, `chore(lfs): ...`, etc. One logical change per commit.
- LaTeX auxiliary files (`.aux`, `.log`, `.synctex.gz`, and friends) are ignored by `.gitignore`.

## Related Materials

The course's full syllabus, French and English course descriptions, and the written examination blueprint live in the Obsidian course folder next to this repository (not in this repo):

- `Syllabus.md` (detailed syllabus, module-by-module timing, design decisions log)
- `Course Description - EN.md` / `Course Description - FR.md`
- `20260731 - Written Examination - Example.md` (2h closed-book exam, question bank)

## License and Authorship

Course materials authored by Riccardo Finotello (CEA) for Polytech Paris-Saclay. The Beamer theme `CEA23` is the property of its upstream maintainers at CEA.
