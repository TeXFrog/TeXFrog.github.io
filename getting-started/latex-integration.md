---
title: LaTeX Integration
layout: default
parent: Getting Started
nav_order: 2
---

# Using TeXFrog in Your LaTeX Paper

TeXFrog proofs compile directly with `pdflatex` using the `texfrog.sty` package. No separate build step is needed — game filtering, diff highlighting, and consolidated figures all happen at compile time.

## Setup

1. Place `texfrog.sty` in the same directory as your `.tex` file (or anywhere TeX can find it).
2. Add `\usepackage[package=cryptocode]{texfrog}` (or `package=nicodemus`) to your preamble.
3. Compile with `pdflatex` as usual.

This works on Overleaf — just upload `texfrog.sty` to your project.

## Rendering Games

Use `\tfrendergame` in the document body to render individual games:

```latex
% Render without highlighting (first game, or when you want a clean version)
\tfrendergame{indcpa}{G0}

% Render with changes highlighted relative to another game
\tfrendergame[diff=G0]{indcpa}{G1}
```

By default, `\tfrendergame` renders without highlighting. The `diff=target` option enables highlighting of lines that changed relative to the target game.

## Rendering Consolidated Figures

Use `\tfrenderfigure` to show multiple games in a single view:

```latex
\begin{figure}[ht]
\centering
\tfrenderfigure{indcpa}{G0,G1,G2,G3}
\caption{Games $G_0$--$G_3$ compared.}
\end{figure}
```

In the consolidated figure, lines that appear in all selected games are output verbatim. Lines that appear in only some games are annotated with `\tfgamelabel{G1,G3}{line content}`.

## Customizing the Highlight Macro

Changed lines are wrapped in `\tfchanged{content}`. The default definition depends on the package profile:

- **cryptocode**: `\providecommand{\tfchanged}[1]{\colorbox{blue!15}{$#1$}}`
- **nicodemus**: `\providecommand{\tfchanged}[1]{\colorbox{blue!15}{#1}}`

This renders changed lines with a light blue background. Because `\providecommand` is used, you can override it in your paper preamble with `\renewcommand`:

```latex
% Use a yellow background instead:
\renewcommand{\tfchanged}[1]{\colorbox{yellow!30}{$#1$}}
```

Or suppress highlighting entirely (useful for the final paper version where you don't want colored boxes):

```latex
\renewcommand{\tfchanged}[1]{#1}
```

**Note on math mode:** For `cryptocode`, the default macro wraps content in `$...$` because the pseudocode content is in math mode and `\colorbox` operates in text mode. For `nicodemus`, content is already in text mode, so no `$...$` wrapping is needed. If you write a custom `\tfchanged`, match the mode of your pseudocode package.

## Customizing the Game Label Macro

In consolidated figures, lines that appear in only some of the selected games are annotated with `\tfgamelabel{labels}{content}`. The default depends on the package profile:

- **cryptocode**: `\providecommand{\tfgamelabel}[2]{#2 \pccomment{#1}}` — appends a `\pccomment` annotation
- **nicodemus**: `\providecommand{\tfgamelabel}[2]{#2}` — no annotation (nicodemus has no built-in comment macro)

Override with `\renewcommand` to change the appearance:

```latex
% Use a margin note instead:
\renewcommand{\tfgamelabel}[2]{#2\marginpar{\tiny #1}}
```

## Referencing Game Names

`\tfgamename{source}{label}` expands to the display name defined via `\tfgamename` in the preamble, wrapped in `\ensuremath`. Use it anywhere in your paper to reference a game by its label:

```latex
In \tfgamename{indcpa}{G1}, we replace the PRF call with a uniformly random value.
```

This works in both text mode and math mode:

```latex
% Text mode — \ensuremath enters math for you:
The advantage gap between \tfgamename{indcpa}{G0} and \tfgamename{indcpa}{G1} is bounded by \ldots

% Math mode — \ensuremath is a no-op:
$x = \tfgamename{indcpa}{G1}$
```

Inside a `tfsource` body, the 1-argument form `\tfgamename{label}` is available as a shorthand that looks up the name from the active source.

## The HTML Viewer

Running `texfrog html build` or `texfrog html serve` (via the Python CLI) produces a standalone HTML site — no integration with LaTeX required. The viewer:

- Shows each game as a rendered SVG, compiled via `pdflatex → pdfcrop → pdftocairo`
- Highlights changed lines with a colored box
- Displays game names (rendered with MathJax) and descriptions in the sidebar
- Supports keyboard navigation (left/right arrow keys) and URL hash links (`#G1`)
- Renders commentary as SVG images using the same LaTeX → PDF → SVG pipeline as game pseudocode

The HTML output is self-contained in its directory and can be served from any static file host, or opened directly in a browser.

**Commentary rendering:** Because commentary is compiled through LaTeX (not MathJax), any commands or environments used in commentary must be defined in your macros file. For example, if your commentary uses `\begin{claim}...\end{claim}`, your macros file must include `\newtheorem{claim}{Claim}`. The packages available in the HTML compilation wrapper include your selected pseudocode package plus `amsfonts`, `amsmath`, `amsthm`, `adjustbox`, and `xcolor`. Additional packages can be added via `\tfpreamble`.

### System Requirements for HTML

- `pdflatex` and your macro files (same TeX setup as your paper)
- `pdfcrop` (from TeX Live) — recommended for clean SVG margins
- `pdftocairo` (from poppler, `brew install poppler` on macOS) or `pdf2svg`

### Troubleshooting HTML Build

For a full list of common errors and solutions, see [Troubleshooting & FAQ]({{ site.baseurl }}/getting-started/troubleshooting/#html-build-issues).
