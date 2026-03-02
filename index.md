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

**Key idea:** Write your pseudocode once in a single source file. Tag each line with the games it belongs to using `%:tags:` comments. TeXFrog produces:

- Individual per-game `.tex` files with changed lines automatically highlighted
- Consolidated comparison figures showing multiple games side by side
- An interactive HTML viewer for navigating the proof in a browser

All from that one source file. TeXFrog currently supports the [`cryptocode`](https://ctan.org/pkg/cryptocode) and [`nicodemus`](https://github.com/awslabs/nicodemus) pseudocode packages.

<br>

## Live demo

[![TeXFrog HTML proof viewer]({{ site.baseurl }}/assets/images/screenshot-web.png)]({{ site.baseurl }}/demos/tutorial-cryptocode/){:target="_blank"}
