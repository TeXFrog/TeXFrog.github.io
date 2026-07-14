---
title: "Tutorial: algpseudocodex"
layout: default
parent: Examples
nav_order: 3
has_children: true
---

# TeXFrog Tutorial (algpseudocodex)

[View interactive demo]({{ site.baseurl }}/demos/tutorial-algpseudocodex/){: .btn .btn-primary target="_blank"}

{: .note-title }
> Package
>
> This tutorial uses [`algpseudocodex`](https://ctan.org/pkg/algpseudocodex). For the same proof using `cryptocode` (the default, with a more detailed walkthrough), see [Tutorial: cryptocode]({% link examples/tutorial-cryptocode/index.md %}).

This tutorial contains the same IND-CPA proof as the cryptocode tutorial, rewritten for the `algpseudocodex` pseudocode package. Comparing the two shows the key syntax differences.

## The Proof Scenario

**Scheme.** A symmetric encryption scheme:

```
Enc(k, m)  =  (r, PRF(k, r) XOR m)    where r is fresh per call
Dec(k, (r, c))  =  PRF(k, r) XOR c
```

**Theorem.** `Enc` is IND-CPA secure if `PRF` is a secure pseudorandom function.

**Proof.** Via a three-hop game sequence:

```
G0 (Real)  ~_PRF  G1  ~_birthday  G2  =  G3 (Ideal)
```

| Game | What changes |
|------|-------------|
| G0 | Oracle computes `y <- PRF(k, r)`, returns `(r, y XOR m_b)` |
| G1 | Oracle computes `y <- RF(r)` (truly random function), returns `(r, y XOR m_b)` |
| G2 | Oracle samples `y` at random, returns `(r, y XOR m_b)` |
| G3 | Oracle samples `c` directly (message not used) |
| Red1 | Reduction: replaces PRF call by querying an external challenger |

G0 to G1 is by PRF security (via Red1). G1 to G2 is by a birthday bound on nonce collisions. G2 to G3 is a perfect equivalence.

## Files

| File | Purpose |
|------|---------|
| `main.tex` | Single source file: declares games, contains pseudocode with `\tfonly` tags, and renders output |
| `macros.tex` | Short macro definitions (no external dependencies) |
| `commentary/*.tex` | Per-game commentary files (LaTeX) |

Unlike `nicodemus`, `algpseudocodex` is [on CTAN](https://ctan.org/pkg/algpseudocodex), so it ships with any modern TeX distribution — there is no `.sty` file to bundle.

See the [Source Files]({% link examples/tutorial-algpseudocodex/source-files.md %}) page for the full contents of `main.tex`.

---

## Key Differences from the cryptocode Tutorial

The `.tex` file structure is the same (game registration, `tfsource` environment, rendering commands). The main differences are in the pseudocode syntax:

1. **`\usepackage[package=algpseudocodex]{texfrog}`** selects the algpseudocodex package profile.
2. **`\usepackage{algpseudocodex}`** loads the package itself (available on CTAN, no bundling needed).

The source syntax differs substantially:

| cryptocode | algpseudocodex |
|-----------|-----------|
{% raw %}| `\begin{pcvstack}[boxed]` | `\begin{algorithmic}[1]` |{% endraw %}
| `\procedure[linenumbering]{Name}{` | `\Procedure{Name}{}` |
| `k \getsr \{0,1\}^\lambda \\` | `\State $k \getsr \{0,1\}^\lambda$` |
| `\pcreturn (b' = b)` | `\State \Return $(b' = b)$` |
| `}` (closing procedure) | `\EndProcedure` |

**Key points:**
- **Text mode**: algpseudocodex environments are text-mode, so math content needs explicit `$...$`.
- **`\State` prefix**: Each pseudocode line starts with `\State` (or `\Statex`). The `\State` is kept outside `\tfchanged{}` because algorithmicx's `\State` does real vertical-mode box bookkeeping that breaks if nested inside the highlight box.
- **No `\\` separators**: `\State`/`\Statex` handle line breaks.
- **`\Procedure`/`\EndProcedure`**: Procedure headers use `\Procedure{Name}{args}` and are closed with `\EndProcedure`. Like cryptocode's `\procedure{...}{` syntax, `\Procedure` lines are never wrapped in `\tfchanged`.

---

## The Proof Source (`main.tex`)

### Lines with no tag appear in every game

```latex
\State $b \getsr \{0,1\}$
\State $b' \gets \Adversary^{\mathsf{LR}}()$
\State \Return $(b' = b)$
```

### Lines with a tag appear only in named games

```latex
\tfonly{G0}{\State $k \getsr \{0,1\}^\lambda$}
```

### Consecutive variant lines encode "slots"

The `y` computation is a four-way slot:

```latex
\tfonly{G0}{\State $y \gets \mathrm{PRF}(k, r)$}
\tfonly{G1}{\State $y \gets \RF(r)$}
\tfonly{G2}{\State $y \getsr \{0,1\}^\lambda$}
\tfonly{Red1}{\State $y \gets \OPRF(r)$}
```

For each game, at most one of these lines survives filtering. They are consecutive, so the chosen line always appears at the right position.

### Procedure headers

In algpseudocodex, procedure headers use `\Procedure{Name}{args}` (closed with `\EndProcedure`). The starred `\tfonly*` forms make the right header appear per game:

{% raw %}
```latex
\Procedure{%
  \tfonly*{G0}{$\INDCPA_\Enc^\Adversary()$}%
  \tfonly*{G1}{Game~$\tfgamename{G1}$}%
}{}
```
{% endraw %}

---

## Running the Tutorial

### Compiling with pdflatex (no Python needed)

The `.tex` file compiles directly with `pdflatex`. You just need `texfrog.sty` in the same directory (algpseudocodex itself comes with your TeX distribution):

```bash
cd examples/tutorial-algpseudocodex
pdflatex main.tex
```

This also works on Overleaf — upload `texfrog.sty`, `main.tex`, `macros.tex`, and the `commentary/` files to a project and compile.

### Building the HTML viewer (requires Python CLI)

If you have the Python CLI installed, you can also build an interactive HTML viewer:

```bash
# Build an interactive HTML viewer
texfrog html build examples/tutorial-algpseudocodex/main.tex -o /tmp/tf_tutorial_algp_html

# Or build and open in your browser with live reload
texfrog html serve examples/tutorial-algpseudocodex/main.tex
```

Or [view the pre-built interactive demo]({{ site.baseurl }}/demos/tutorial-algpseudocodex/){:target="_blank"}.

---

## What `\tfchanged` Looks Like

The default highlight macro for algpseudocodex is text-mode (like nicodemus), with the `\State` prefix kept outside the highlight box:

```latex
\State \tfchanged{$y \getsr \{0,1\}^\lambda$}
```

No `$...$` wrapping of the whole line — unlike cryptocode, algpseudocodex content is text-mode, and each math fragment carries its own `$...$`.

---

## Next Steps

- [Writing a Proof]({% link getting-started/writing-proofs.md %}) — full reference for the `.tex` input format
- [Tutorial: cryptocode]({% link examples/tutorial-cryptocode/index.md %}) — the same proof using `cryptocode` (with a more detailed walkthrough)
- [Tutorial: nicodemus]({% link examples/tutorial-nicodemus/index.md %}) — the same proof using the `nicodemus` package
- [LaTeX Integration]({% link getting-started/latex-integration.md %}) — customizing `\tfchanged` and `\tfgamelabel`
