---
title: Examples
layout: default
nav_order: 3
has_children: true
has_toc: false
---

# Examples

All examples compile directly with `pdflatex` — no Python needed. Just place `texfrog.sty` in the same directory.

The [TeXFrog Tutorial]({% link examples/tutorial/index.md %}) implements the same IND-CPA game-hopping proof in all three pseudocode packages — `cryptocode`, `nicodemus`, and `algpseudocodex` — with tabs to switch between them so you can compare the syntax side by side. It proves IND-CPA security of a PRF-based symmetric encryption scheme via a three-hop game sequence with 4 games and 1 reduction.

We also have a demo of TeXFrog for a key exchange model, specifically a signed-Diffie–Hellman key exchange protocol. This is extracted from Section 3.1 of a [technical report](https://github.com/proof-ladders/protocol-ladder/blob/main/Notes/computational/main.pdf) by Doreen Riepel and Paul Rösler for the [proof-ladders project](https://github.com/proof-ladders/).

| Example | Pseudocode Package | Source Files | Live Demo |
|----------|---------|--------------|-----------|
| [Tutorial: cryptocode quickstart](https://github.com/TeXFrog/TeXFrog/tree/main/examples/tutorial-cryptocode-quickstart) | `cryptocode` | N/A | N/A |
| [TeXFrog Tutorial]({% link examples/tutorial/index.md %}) | `cryptocode` · `nicodemus` · `algpseudocodex` | [Source]({% link examples/tutorial/source-files.md %}) | [cryptocode]({{ site.baseurl }}/demos/tutorial-cryptocode/){:target="_blank"} · [nicodemus]({{ site.baseurl }}/demos/tutorial-nicodemus/){:target="_blank"} · [algpseudocodex]({{ site.baseurl }}/demos/tutorial-algpseudocodex/){:target="_blank"} |
| [Multiple proofs](https://github.com/TeXFrog/TeXFrog/tree/main/examples/example-multiproof) | `cryptocode` | N/A | N/A |
| [Signed DH proof](https://github.com/proof-ladders/protocol-ladder/blob/main/Notes/computational/main.pdf){:target="_blank" } | `nicodemus` | N/A | [View demo]({{ site.baseurl }}/demos/example-proof-ladders-signed-dh/){:target="_blank"} |

{: .warning-title }
> Warning
>
> The TeXFrog version of the Signed DH proof was generated automatically by having an LLM extract the metadata about which lines belong in which games/reductions from the LaTeX source code in the authors' paper. On the one hand, it's awesome that this can be done automatically. On the other hand, this extraction has not been checked for correctness against the original authors' paper, so don't rely on it for scientific purposes; it's here solely for visual demonstration purposes.
