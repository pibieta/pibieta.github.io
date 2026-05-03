# pibieta.github.io

Source for my personal technical blog, served at <https://pibieta.github.io>.
Built with [Quarto](https://quarto.org); rendered HTML lives on the `gh-pages`
branch and is served by GitHub Pages.

## Local setup

1. **Install Quarto** (>= 1.5):
   <https://quarto.org/docs/get-started/>

2. **Create a Python env** (any tool works; this is one option):

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. **Preview the site** with live reload:

   ```bash
   quarto preview
   ```

   Opens at `http://localhost:4848`.

4. **Render once** (writes to `_site/`):

   ```bash
   quarto render
   ```

## First push to GitHub

The repo name **must** be `pibieta.github.io` for GitHub to serve it from
the user-site URL.

```bash
cd pibieta.github.io
git init
git add .
git commit -m "Initial commit: Quarto blog scaffold"
git branch -M main
git remote add origin https://github.com/pibieta/pibieta.github.io.git
git push -u origin main
```

Then on GitHub:

1. Go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set branch to **`gh-pages`** and folder to **`/ (root)`**. Save.

The first push triggers `.github/workflows/publish.yml`, which:

- installs Quarto and Python,
- runs `quarto render`,
- pushes the rendered `_site/` to the `gh-pages` branch.

After the workflow finishes (~2–3 min), the site is live at
<https://pibieta.github.io>.

## Writing a new post

```bash
mkdir -p posts/my-new-post
$EDITOR posts/my-new-post/index.qmd
```

Minimum front matter:

```yaml
---
title: "My new post"
description: "One-sentence summary used on the listing page."
date: 2026-05-09
categories: [machine-learning, simulation]
draft: false
---
```

Set `draft: true` to keep it out of the listing while you work on it.

### Math

Inline: `$X \sim \mathcal{N}(0, 1)$` → $X \sim \mathcal{N}(0, 1)$.

Display:

```latex
$$
\hat{\beta}_{\mathrm{OLS}} = (X^\top X)^{-1} X^\top y
$$
```

### Executable Python

````markdown
```{python}
import numpy as np
np.random.default_rng(0).normal(size=5)
```
````

The chunk runs at render time; output is captured into the page.

### Citations

Add a BibTeX entry to `references.bib`, then cite with `[@key]` in the post.
A `## References` section with `::: {#refs} :::` anywhere in the post
auto-fills the bibliography for that post.

### Theorems / proofs / asides

```markdown
::: {.callout-note title="Theorem (Cauchy–Schwarz)" appearance="simple"}
For random variables $X, Y$ with finite second moments,
$|\mathbb{E}[XY]|^2 \le \mathbb{E}[X^2]\, \mathbb{E}[Y^2]$.
:::
```

`note`, `tip`, `warning`, `caution`, `important` are all available.

## Repo layout

```
.
├── _quarto.yml                 # site config (theme, navbar, math, citations)
├── index.qmd                   # home page (post listing)
├── about.qmd                   # about page
├── posts/
│   ├── _metadata.yml           # post defaults applied to every post
│   ├── welcome/
│   │   └── index.qmd
│   └── mutual-information-vs-correlation/
│       └── index.qmd           # sample post: math + Python + citations
├── references.bib              # bibliography
├── styles.css                  # CSS overrides
├── theme-light.scss            # light-mode theme
├── theme-dark.scss             # dark-mode theme
├── requirements.txt            # Python deps for executable chunks
├── .nojekyll                   # tells Pages not to run Jekyll
├── .gitignore
└── .github/workflows/publish.yml
```

## Why these choices

- **Quarto over Jekyll/Hugo** — first-class executable code in Python/R/Julia,
  proper citation pipeline, native cross-references, and theorem/callout
  blocks. The right fit for posts that are essentially short papers.
- **KaTeX over MathJax** — much faster to render. Switch to `mathjax` in
  `_quarto.yml` if a post needs `\begin{align*}` or custom macros.
- **Cosmo + Source Serif 4** — readable, uncluttered, vaguely academic.
  All controlled in `theme-light.scss` / `theme-dark.scss`; tweak freely.
