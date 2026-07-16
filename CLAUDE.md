# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

DKlab is a personal Jupyter Book (built via [MyST](https://mystmd.org)) used as a learning log and reference site for AI/ML topics. Content is a mix of MyST markdown pages and Jupyter notebooks, built to static HTML and deployed to GitHub Pages. It is not a software application — there is no app code, package, or test suite; changes are almost always content edits (notebooks, markdown pages, reference PDFs) or MyST/build configuration.

## Commands

Install dependencies:
```bash
pip install -r requirements.txt
npm install -g jupyter-book   # or: mystmd, per CI (see .github/workflows/deploy.yml)
```

Build the site locally (outputs to `_build/html`):
```bash
jupyter-book build --html
```

Live preview while editing:
```bash
jupyter-book start
```

There is no lint or test command in this repo — validation is "does the book build and render correctly."

## Deployment

`.github/workflows/deploy.yml` builds and deploys automatically to GitHub Pages on every push to `main`. It installs `jupyter-book` via npm (Node 18), runs `jupyter-book build --html`, and publishes `_build/html`. `_build/` is a build artifact directory and should not be hand-edited.

## Site structure and how pages are wired up

- `myst.yml` is the project's single source of truth for site config: title, author, GitHub link, licensing (code: MIT, content: CC-BY-4.0), theme (`book-theme`), logo, and — critically — the **table of contents** (`project.toc`). Any new top-level page or notebook must be added to `toc` in `myst.yml` or it will not appear in the built site's navigation, even if the file exists under `notebooks/`.
- `intro.md` is the book's landing page.
- `notebooks/` holds the actual content, organized by topic (e.g. `notebooks/foundation/`, `notebooks/deep_learning/`). These are the notebooks/markdown files referenced from `myst.yml`'s `toc`.
- `notebooks/Pytorch/` contains a separate, numbered course-style sequence of lab notebooks (`1 Getting started/`, `2 Workflow/`, `3 Data management/`, ...), each with paired `helper_utils.py` / `unittests.py` support files. This tree is not currently wired into `myst.yml`'s `toc` — check there before assuming a notebook is published on the site.
- `notebooks/references.bib` + `notebooks/bibliography.md` provide the book's citation/bibliography support via MyST's `{bibliography}` directive.
- `material/` is a personal reference library of source PDFs (textbooks, papers), organized by subject folder (`DL/`, `ML/`, `MLOps/`, `NLP/`, `RL/`, `mml/`). These are not part of the built site — they're background reading material kept alongside the notes.
- `images/` holds image assets referenced from content pages.

## Working with content

- New topic/lesson: add the notebook or `.md` file under `notebooks/<topic>/`, then register it in `myst.yml`'s `project.toc` (as a `file:` entry, nested under a `title:`/`children:` section if it belongs to a topic group).
- MyST markdown supports directives like `{note}`, `{figure}`, `{math}`, `{bibliography}` — see `intro.md` for working examples of admonitions, figures, and equations.
- Since notebooks are the primary content format, prefer editing `.ipynb` files with notebook-aware tools rather than raw text edits, to keep cell outputs/metadata intact.
