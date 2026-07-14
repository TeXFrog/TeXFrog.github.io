---
title: Source Files
layout: default
parent: "TeXFrog Tutorial"
grand_parent: Examples
nav_order: 1
redirect_from:
  - /examples/tutorial-cryptocode/source-files.html
  - /examples/tutorial-nicodemus/source-files.html
  - /examples/tutorial-algpseudocodex/source-files.html
---

# Source Files: TeXFrog Tutorial

The full `main.tex` for the [TeXFrog Tutorial]({% link examples/tutorial/index.md %}),
in each pseudocode package. Switch packages with the tabs — everything above
`\begin{document}` (game registration, descriptions, figures) is identical; only the
`tfsource` block and the `\usepackage` lines differ.

{% raw %}
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
\documentclass{article}

\usepackage[margin=1in,letterpaper]{geometry}
\usepackage[n,advantage,operators,sets,adversary,landau,probability,notions,logic,ff,mm,primitives,events,complexity,oracles,asymptotics,keys]{cryptocode}
\usepackage{amsfonts,amsmath,amsthm}
\usepackage[package=cryptocode]{texfrog}

\input{macros.tex}

\newtheorem{theorem}{Theorem}

%%% Game registration
\tfgames{indcpa}{G0, G1, Red1, G2, G3}
\tfgamename{indcpa}{G0}{G_0}
\tfgamename{indcpa}{G1}{G_1}
\tfgamename{indcpa}{Red1}{\Bdversary_1}
\tfgamename{indcpa}{G2}{G_2}
\tfgamename{indcpa}{G3}{G_3}

\tfreduction{indcpa}{Red1}
\tfrelatedgames{indcpa}{Red1}{G0, G1}

\tfmacrofile{macros.tex}

%%% Proof source
\begin{tfsource}{indcpa}
\begin{pcvstack}[boxed]
  \procedure[linenumbering]{%
    \tfonly*{G0}{Game $\tfgamename{G0} = \INDCPA_\Enc^\Adversary()$}%
    \tfonly*{G1}{Game $\tfgamename{G1}$}%
    \tfonly*{G2}{Game $\tfgamename{G2}$}%
    \tfonly*{G3}{Game $\tfgamename{G3}$}%
    \tfonly*{Red1}{Reduction $\Bdversary_1^{\OPRF}$}%
    \tffigonly{Games $\tfgamename{G0}$--$\tfgamename{G3}$}%
  }{
    \tfonly{G0}{k \getsr \{0,1\}^\lambda \\}
    b \getsr \{0,1\} \\
    b' \gets \Adversary^{\mathsf{LR}}() \\
    \pcreturn (b' = b)
  }
  \pclb
  \procedure[linenumbering]{$\mathsf{LR}(m_0, m_1)$}{
    r \getsr \{0,1\}^\lambda \\
    \tfonly{G0}{y \gets \mathrm{PRF}(k, r) \\}
    \tfonly{G1}{y \gets \RF(r) \\}
    \tfonly{G2}{y \getsr \{0,1\}^\lambda \\}
    \tfonly{Red1}{y \gets \OPRF(r) \\}
    \tfonly{G0-G2}{c \gets y \oplus m_b \\}
    \tfonly{G3}{c \getsr \{0,1\}^\lambda \\}
    \pcreturn (r, c)
  }
\end{pcvstack}
\end{tfsource}

\begin{document}
% ... \tfrendergame / \tfrenderfigure calls, theorem, and commentary ...
\end{document}
```

</div>
<div class="tf-panel" data-pkg="nicodemus">
<p class="panel-tag">nicodemus</p>

```latex
\documentclass{article}

\usepackage[margin=1in,letterpaper]{geometry}
\usepackage{amsfonts,amsmath,amsthm}
\usepackage{xcolor}
\usepackage{nicodemus}
\usepackage[package=nicodemus]{texfrog}

\input{macros.tex}

\newtheorem{theorem}{Theorem}

%%% Game registration
\tfgames{indcpa}{G0, G1, Red1, G2, G3}
\tfgamename{indcpa}{G0}{G_0}
\tfgamename{indcpa}{G1}{G_1}
\tfgamename{indcpa}{Red1}{\Bdversary_1}
\tfgamename{indcpa}{G2}{G_2}
\tfgamename{indcpa}{G3}{G_3}

\tfreduction{indcpa}{Red1}
\tfrelatedgames{indcpa}{Red1}{G0, G1}

\tfmacrofile{macros.tex}
\tfmacrofile{nicodemus.sty}

%%% Proof source
\begin{tfsource}{indcpa}
\begin{tabular}[t]{l}
	\nicodemusboxNew{250pt}{%
		\tfonly*{G0}{\nicodemusheader{$\INDCPA_\Enc^\Adversary()$}}
		\tfonly*{G1}{\nicodemusheader{Game~$\tfgamename{G1}$}}
		\tfonly*{G2}{\nicodemusheader{Game~$\tfgamename{G2}$}}
		\tfonly*{G3}{\nicodemusheader{Game~$\tfgamename{G3}$}}
		\tfonly*{Red1}{\nicodemusheader{Reduction $\Bdversary_1^{\OPRF}$}}
		\tffigonly{\nicodemusheader{Games $\tfgamename{G0}$--$\tfgamename{G3}$}}
		\begin{nicodemus}
			\tfonly{G0}{\item $k \getsr \{0,1\}^\lambda$}
			\item $b \getsr \{0,1\}$
			\item $b' \gets \Adversary^{\mathsf{LR}}()$
			\item Return $(b' = b)$
		\end{nicodemus}%
		\medskip
		\nicodemusheader{Oracle $\mathsf{LR}(m_0, m_1)$}
		\begin{nicodemus}
			\item $r \getsr \{0,1\}^\lambda$
			\tfonly{G0}{\item $y \gets \mathrm{PRF}(k, r)$}
			\tfonly{G1}{\item $y \gets \RF(r)$}
			\tfonly{G2}{\item $y \getsr \{0,1\}^\lambda$}
			\tfonly{Red1}{\item $y \gets \OPRF(r)$}
			\tfonly{G0-G2}{\item $c \gets y \oplus m_b$}
			\tfonly{G3}{\item $c \getsr \{0,1\}^\lambda$}
			\item Return $(r, c)$
		\end{nicodemus}%
	}%
\end{tabular}%
\end{tfsource}

\begin{document}
% ... \tfrendergame / \tfrenderfigure calls, theorem, and commentary ...
\end{document}
```

</div>
<div class="tf-panel" data-pkg="algpseudocodex">
<p class="panel-tag">algpseudocodex</p>

```latex
\documentclass{article}

\usepackage[margin=1in,letterpaper]{geometry}
\usepackage{amsfonts,amsmath,amsthm}
\usepackage{xcolor}
\usepackage{algpseudocodex}
\usepackage[package=algpseudocodex]{texfrog}

\input{macros.tex}

\newtheorem{theorem}{Theorem}

%%% Game registration
\tfgames{indcpa}{G0, G1, Red1, G2, G3}
\tfgamename{indcpa}{G0}{G_0}
\tfgamename{indcpa}{G1}{G_1}
\tfgamename{indcpa}{Red1}{\Bdversary_1}
\tfgamename{indcpa}{G2}{G_2}
\tfgamename{indcpa}{G3}{G_3}

\tfreduction{indcpa}{Red1}
\tfrelatedgames{indcpa}{Red1}{G0, G1}

\tfmacrofile{macros.tex}

%%% Proof source
\begin{tfsource}{indcpa}
\begin{algorithmic}[1]
\Procedure{%
  \tfonly*{G0}{$\INDCPA_\Enc^\Adversary()$}%
  \tfonly*{G1}{Game~$\tfgamename{G1}$}%
  \tfonly*{G2}{Game~$\tfgamename{G2}$}%
  \tfonly*{G3}{Game~$\tfgamename{G3}$}%
  \tfonly*{Red1}{Reduction $\Bdversary_1^{\OPRF}$}%
  \tffigonly{Games $\tfgamename{G0}$--$\tfgamename{G3}$}%
}{}
\tfonly{G0}{\State $k \getsr \{0,1\}^\lambda$}
\State $b \getsr \{0,1\}$
\State $b' \gets \Adversary^{\mathsf{LR}}()$
\State \Return $(b' = b)$
\EndProcedure
\Statex
\Procedure{Oracle $\mathsf{LR}(m_0, m_1)$}{}
\State $r \getsr \{0,1\}^\lambda$
\tfonly{G0}{\State $y \gets \mathrm{PRF}(k, r)$}
\tfonly{G1}{\State $y \gets \RF(r)$}
\tfonly{G2}{\State $y \getsr \{0,1\}^\lambda$}
\tfonly{Red1}{\State $y \gets \OPRF(r)$}
\tfonly{G0-G2}{\State $c \gets y \oplus m_b$}
\tfonly{G3}{\State $c \getsr \{0,1\}^\lambda$}
\State \Return $(r, c)$
\EndProcedure
\end{algorithmic}
\end{tfsource}

\begin{document}
% ... \tfrendergame / \tfrenderfigure calls, theorem, and commentary ...
\end{document}
```

</div>
</div>

{::options parse_block_html="false" /}
{% endraw %}
