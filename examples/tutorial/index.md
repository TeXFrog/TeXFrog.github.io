---
title: "TeXFrog Tutorial"
layout: default
parent: Examples
nav_order: 1
has_children: true
redirect_from:
  - /examples/tutorial-cryptocode/
  - /examples/tutorial-nicodemus/
  - /examples/tutorial-algpseudocodex/
---

# TeXFrog Tutorial

This tutorial walks through one complete IND-CPA game-hopping proof, written three
ways — in [`cryptocode`](https://ctan.org/pkg/cryptocode),
[`nicodemus`](https://github.com/TeXFrog/TeXFrog/blob/main/resources/nicodemus.sty),
and [`algpseudocodex`](https://ctan.org/pkg/algpseudocodex). **Use the tabs on any
code block to switch package** — your choice follows you across every block on the
page (and is remembered between pages), so you can read the whole tutorial in the
package you actually use.

View the pre-built interactive demo in each package:

[cryptocode demo]({{ site.baseurl }}/demos/tutorial-cryptocode/){: .btn .btn-primary target="_blank"}
[nicodemus demo]({{ site.baseurl }}/demos/tutorial-nicodemus/){: .btn target="_blank"}
[algpseudocodex demo]({{ site.baseurl }}/demos/tutorial-algpseudocodex/){: .btn target="_blank"}

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

`cryptocode` and `algpseudocodex` are on CTAN, so they ship with any modern TeX
distribution. `nicodemus` is not on CTAN — it is bundled with TeXFrog, so the
nicodemus version of this tutorial also includes a `nicodemus.sty` file (declared
via `\tfmacrofile{nicodemus.sty}`).

See the [Source Files]({% link examples/tutorial/source-files.md %}) page for the full contents of `main.tex` in each package.

## The Package at a Glance

The `.tex` structure — game registration, the `tfsource` environment, and the
rendering commands — is identical in all three packages. Only the pseudocode
syntax differs:

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

- **Math mode.** Pseudocode lives inside `\procedure` environments, so content is math-mode.
- **Line separator.** Lines end with `\\`.
- **Selected via** `\usepackage[package=cryptocode]{texfrog}` (the default).
- **Highlighting.** `\tfchanged` wraps content in `$...$`; `\tfgamelabel` uses `\pccomment`.

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

- **Text mode.** `nicodemus` environments are text-mode, so math needs explicit `$...$`.
- **Line prefix.** Each line starts with `\item`, kept outside `\tfchanged{}` to preserve list structure.
- **Selected via** `\usepackage[package=nicodemus]{texfrog}`, plus `\tfmacrofile{nicodemus.sty}` (bundled with TeXFrog; not on CTAN).
- **Highlighting.** `\tfchanged` wraps content directly; `\tfgamelabel` has no comment macro.

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

- **Text mode.** `algorithmic` environments are text-mode, so math needs explicit `$...$`.
- **Line prefix.** Each line starts with `\State` (or `\Statex`), kept outside `\tfchanged{}` (algorithmicx's `\State` does vertical-mode bookkeeping that breaks inside a box).
- **Selected via** `\usepackage[package=algpseudocodex]{texfrog}`.
- **Highlighting.** `\tfchanged` wraps content directly; `\tfgamelabel` uses `\Comment`.

</div>
</div>

{::options parse_block_html="false" /}

## The Source File (`main.tex`)

The `.tex` file is the single source of truth: it compiles directly with `pdflatex`
(via `texfrog.sty`) and is also parsed by the Python CLI for HTML export.

### Lines with no tag appear in every game

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

```latex
b \getsr \{0,1\} \\
b' \gets \Adversary^{\mathsf{LR}}() \\
\pcreturn (b' = b)
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```latex
\item $b \getsr \{0,1\}$
\item $b' \gets \Adversary^{\mathsf{LR}}()$
\item Return $(b' = b)$
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```latex
\State $b \getsr \{0,1\}$
\State $b' \gets \Adversary^{\mathsf{LR}}()$
\State \Return $(b' = b)$
```

</div>
</div>

{::options parse_block_html="false" /}

### Lines with a tag appear only in named games

Lines inside `\tfonly{tags}{content}` appear only in the listed games. The tag
syntax (`G0`, `G0,G1`, ranges like `G1-G3`) is identical across packages:

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

```latex
\tfonly{G0,G1-G3}{k \getsr \{0,1\}^\lambda \\}
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```latex
\tfonly{G0,G1-G3}{\item $k \getsr \{0,1\}^\lambda$}
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```latex
\tfonly{G0,G1-G3}{\State $k \getsr \{0,1\}^\lambda$}
```

</div>
</div>

{::options parse_block_html="false" /}

### Consecutive variant lines encode "slots"

**The most important structural rule:** variant lines for the same logical slot must
be consecutive in the source. TeXFrog filters but never reorders. The `y` computation
is a four-way slot:

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

```latex
\tfonly{G0}{y \gets \mathrm{PRF}(k, r) \\}
\tfonly{G1}{y \gets \RF(r) \\}
\tfonly{G2}{y \getsr \{0,1\}^\lambda \\}
\tfonly{Red1}{y \gets \OPRF(r) \\}
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```latex
\tfonly{G0}{\item $y \gets \mathrm{PRF}(k, r)$}
\tfonly{G1}{\item $y \gets \RF(r)$}
\tfonly{G2}{\item $y \getsr \{0,1\}^\lambda$}
\tfonly{Red1}{\item $y \gets \OPRF(r)$}
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```latex
\tfonly{G0}{\State $y \gets \mathrm{PRF}(k, r)$}
\tfonly{G1}{\State $y \gets \RF(r)$}
\tfonly{G2}{\State $y \getsr \{0,1\}^\lambda$}
\tfonly{Red1}{\State $y \gets \OPRF(r)$}
```

</div>
</div>

{::options parse_block_html="false" /}

For each game, at most one of these lines survives filtering. Because they are
consecutive, the chosen line always appears at the right position.

### Procedure headers

Procedure headers use the starred `\tfonly*` form so the right header appears in each
individual game but is suppressed in consolidated figures (`\tffigonly` supplies the
combined header). Header lines are never wrapped in `\tfchanged`:

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

{% raw %}
```latex
\procedure[linenumbering]{%
  \tfonly*{G0}{Game $\tfgamename{G0}$}%
  \tfonly*{G1}{Game $\tfgamename{G1}$}%
  \tffigonly{Games $\tfgamename{G0}$--$\tfgamename{G3}$}%
}{ ... }
```
{% endraw %}

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

{% raw %}
```latex
\tfonly*{G0}{\nicodemusheader{$\INDCPA_\Enc^\Adversary()$}}
\tfonly*{G1}{\nicodemusheader{Game~$\tfgamename{G1}$}}
\tffigonly{\nicodemusheader{Games $\tfgamename{G0}$--$\tfgamename{G3}$}}
```
{% endraw %}

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

{% raw %}
```latex
\Procedure{%
  \tfonly*{G0}{$\INDCPA_\Enc^\Adversary()$}%
  \tfonly*{G1}{Game~$\tfgamename{G1}$}%
  \tffigonly{Games $\tfgamename{G0}$--$\tfgamename{G3}$}%
}{}
```
{% endraw %}

</div>
</div>

{::options parse_block_html="false" /}

### Rendering

The rendering commands are the same in every package:

```latex
\tfrendergame{indcpa}{G0}                     % no highlighting
\tfrendergame[diff=G0]{indcpa}{G1}            % changes from G0 highlighted
\tfrenderfigure{indcpa}{G0,G1,G2,G3,Red1}     % consolidated figure
```

## Running the Tutorial

### Compiling with pdflatex (no Python needed)

The `.tex` file compiles directly with `pdflatex` — you just need `texfrog.sty` in the
same directory (and, for the nicodemus version, `nicodemus.sty`):

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

```bash
cd examples/tutorial-cryptocode
pdflatex main.tex
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```bash
cd examples/tutorial-nicodemus
pdflatex main.tex
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```bash
cd examples/tutorial-algpseudocodex
pdflatex main.tex
```

</div>
</div>

{::options parse_block_html="false" /}

This also works on Overleaf — upload `texfrog.sty`, `main.tex`, `macros.tex`, the
`commentary/` files (and `nicodemus.sty` for the nicodemus version) to a project and compile.

### Building the HTML viewer (requires Python CLI)

With the Python CLI installed, build an interactive HTML viewer:

{::options parse_block_html="true" /}

<div class="tf-tabs">
<div class="tabrow" role="tablist" aria-label="pseudocode package">
<button type="button" role="tab" data-pkg="cryptocode">cryptocode</button>
<button type="button" role="tab" data-pkg="nicodemus">nicodemus</button>
<button type="button" role="tab" data-pkg="algpseudocodex">algpseudocodex</button>
</div>
<div class="tf-panel" data-pkg="cryptocode">
<p class="panel-tag">cryptocode</p>

```bash
texfrog html serve examples/tutorial-cryptocode/main.tex
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```bash
texfrog html serve examples/tutorial-nicodemus/main.tex
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```bash
texfrog html serve examples/tutorial-algpseudocodex/main.tex
```

</div>
</div>

{::options parse_block_html="false" /}

Or view the pre-built demos:
[cryptocode]({{ site.baseurl }}/demos/tutorial-cryptocode/){:target="_blank"} ·
[nicodemus]({{ site.baseurl }}/demos/tutorial-nicodemus/){:target="_blank"} ·
[algpseudocodex]({{ site.baseurl }}/demos/tutorial-algpseudocodex/){:target="_blank"}.

## Next Steps

- [Writing a Proof]({% link getting-started/writing-proofs.md %}) — full reference for the `.tex` input format
- [LaTeX Integration]({% link getting-started/latex-integration.md %}) — customizing `\tfchanged` and `\tfgamelabel`
- [Source Files]({% link examples/tutorial/source-files.md %}) — full `main.tex` in each package
