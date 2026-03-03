---
title: Examples
layout: default
nav_order: 3
has_children: true
has_toc: false
---

# Examples

TeXFrog ships with two tutorials that implement the same IND-CPA game-hopping proof using different pseudocode packages. Comparing them side by side shows the syntax differences between `cryptocode` and `nicodemus`.

Both tutorials prove IND-CPA security of a PRF-based symmetric encryption scheme via a three-hop game sequence with 4 games and 1 reduction.

We also have a demo of TeXFrog for a key exchange model, specifically a signed-Diffie–Hellman key exchange protocol. This is extracted from Section 3.1 of a [technical report](https://github.com/proof-ladders/protocol-ladder/blob/main/Notes/computational/main.pdf) by Doreen Riepel and Paul Rösler for the [proof-ladders project](https://github.com/proof-ladders/).

| Example | Pseudocode Package | Source Files | Live Demo |
|----------|---------|--------------|-----------|
| [Tutorial: cryptocode]({{ site.baseurl }}/examples/tutorial-cryptocode/) | `cryptocode` | [Source]({{ site.baseurl }}/examples/tutorial-cryptocode/source-files.html) | [View demo]({{ site.baseurl }}/demos/tutorial-cryptocode/){:target="_blank"} |
| [Tutorial: nicodemus]({{ site.baseurl }}/examples/tutorial-nicodemus/) | `nicodemus` | [Source]({{ site.baseurl }}/examples/tutorial-nicodemus/source-files.html) | [View demo]({{ site.baseurl }}/demos/tutorial-nicodemus/){:target="_blank"} |
| [Signed DH proof](https://github.com/proof-ladders/protocol-ladder/blob/main/Notes/computational/main.pdf){:target="_blank" } | `nicodemus` | N/A | [View demo]({{ site.baseurl }}/demos/example-proof-ladders-signed-dh/){:target="_blank"} |

{: .warning-title }
> Warning
>
> The TeXFrog version of the Signed DH proof was generated automatically by having an LLM extract the metadata about which lines belong in which games/reductions from the LaTeX source code in the authors' paper. On the one hand, it's awesome that this can be done automatically. On the other hand, this extraction has not been checked for correctness against the original authors' paper, so don't rely on it for scientific purposes; it's here solely for visual demonstration purposes.
