---
title: Home
layout: home
nav_order: 1
---

# TeXFrog

<br>

{: .note-title }
> Note
>
> TeXFrog is an early-stage tool under active development. The input format, command-line interface, and output may change as the design evolves. [Feedback, suggestions, and contributions]({{ site.baseurl }}/getting-started/#contributing) are very welcome.

<br>

TeXFrog helps cryptographers manage game-hopping proofs in LaTeX. If you have ever maintained a dozen nearly-identical game files by hand, copying lines between them and trying to keep highlights consistent, TeXFrog is meant to solve that problem.

**Key idea:** Write your pseudocode once in a single `.tex` file. Tag content with the games it belongs to using `\tfonly{games}{content}` commands. TeXFrog produces:

- Individual per-game renderings with changed lines automatically highlighted (via `pdflatex` — no extra tools needed)
- Consolidated comparison figures showing multiple games side by side (via `pdflatex`)
- An interactive HTML viewer for navigating the proof in a browser (via the optional Python CLI)

All from that one source file. TeXFrog currently supports the [`cryptocode`](https://ctan.org/pkg/cryptocode) and [`nicodemus`](https://github.com/awslabs/nicodemus) pseudocode packages, and we are open to supporting others.

<br>

## Live demos

[![TeXFrog HTML proof viewer]({{ site.baseurl }}/assets/images/screenshot-web.png)]({{ site.baseurl }}/examples/){:target="_blank"}
