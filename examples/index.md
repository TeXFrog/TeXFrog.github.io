---
title: Examples
layout: default
nav_order: 3
has_children: true
has_toc: false
---

# Examples

TeXFrog ships with two tutorials that implement the same IND-CPA game-hopping proof using different pseudocode packages. Comparing them side by side shows the syntax differences between `cryptocode` and `nicodemus`.

| Tutorial | Pseudocode Package | Source Files | Live Demo |
|----------|---------|--------------|-----------|
| [Tutorial: cryptocode]({{ site.baseurl }}/examples/tutorial-cryptocode/) | `cryptocode` | [Source]({{ site.baseurl }}/examples/tutorial-cryptocode/source-files.html) | [View demo]({{ site.baseurl }}/demos/tutorial-cryptocode/){:target="_blank"} |
| [Tutorial: nicodemus]({{ site.baseurl }}/examples/tutorial-nicodemus/) | `nicodemus` | [Source]({{ site.baseurl }}/examples/tutorial-nicodemus/source-files.html) | [View demo]({{ site.baseurl }}/demos/tutorial-nicodemus/){:target="_blank"} |

Both tutorials prove IND-CPA security of a PRF-based symmetric encryption scheme via a two-hop game sequence with 3 games and 1 reduction.
