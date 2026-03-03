---
title: Source Files
layout: default
parent: "Tutorial: cryptocode"
grand_parent: Examples
nav_order: 1
---

# Source Files: cryptocode Tutorial

These are the input files for the [cryptocode tutorial]({{ site.baseurl }}/examples/tutorial-cryptocode/).

## proof.yaml

```yaml
# TeXFrog tutorial proof: IND-CPA security of PRF-based symmetric encryption.
#
# Scheme: Enc(k, m) = (r, PRF(k, r) xor m),  r fresh per encryption.
#
# This tutorial example has 4 games and 1 reduction.  It illustrates:
#   - Lines that appear in every game (no %:tags: comment in source.tex)
#   - Lines that appear only in some games (with %:tags:)
#   - Tag ranges: G0-G2 means positions 0 through the position of G2 (resolved by position)
#   - A reduction that shares pseudocode structure with its adjacent games

macros:
  - macros.tex

source: games_source.tex

games:
  - label: G0
    latex_name: 'G_0'
    description: '$\INDCPA_\Enc^\Adversary()$: The IND-CPA game. The LR oracle encrypts using the actual PRF.'

  - label: G1
    latex_name: 'G_1'
    description: 'Replace $\mathrm{PRF}(k, r)$ with a truly random function $\RF(r)$.'

  - label: Red1
    latex_name: '\Bdversary_1'
    description: 'Reduction against PRF security, simulating Games \tfgamename{G0} and \tfgamename{G1} via an external PRF challenger.'
    reduction: true
    related_games: [G0, G1]

  - label: G2
    latex_name: 'G_2'
    description: 'Replace $\RF(r)$ with independently sampled randomness.'

  - label: G3
    latex_name: 'G_3'
    description: 'Ciphertext is uniformly random, independent of $m_b$.'

commentary:
  G0: commentary/G0.tex
  G1: commentary/G1.tex
  Red1: commentary/Red1.tex
  G2: commentary/G2.tex
  G3: commentary/G3.tex

figures:
  - label: all_games
    games: "G0,G1,G2,G3,Red1"
    procedure_name: "Games $\\tfgamename{G0}$--$\\tfgamename{G3}$, Reduction $\\tfgamename{Red1}$"
```

## games_source.tex

```latex
% TeXFrog tutorial source: IND-CPA proof for PRF-based symmetric encryption.
%
% Scheme: Enc(k, m) = (r, PRF(k, r) xor m),  where r is sampled fresh each call.
%
% Tag syntax: %:tags: label1,label2-label3
% - Lines with no %:tags: comment appear in every game/reduction.
% - Lines tagged with one or more labels appear only in those games/reductions.
% - Ranges like G0-G2 are resolved by POSITION in the 'games:' list, not alphabetically.
% - Variant lines for the same "slot" must be consecutive in this file.

\begin{pcvstack}[boxed]

%%% -- Challenge experiment procedure header (one per game/reduction) -------------

    \procedure[linenumbering]{Game $\tfgamename{G0} = \INDCPA_\Enc^\Adversary()$}{ %:tags: G0
    \procedure[linenumbering]{Game $\tfgamename{G1}$}{ %:tags: G1
    \procedure[linenumbering]{Game $\tfgamename{G2}$}{ %:tags: G2
    \procedure[linenumbering]{Game $\tfgamename{G3}$}{ %:tags: G3
    \procedure[linenumbering]{Reduction $\Bdversary_1^{\OPRF}$}{ %:tags: Red1

%%% -- Challenge experiment body -------------------------------------------------

        % PRF key: only Game 1
        k \getsr \{0,1\}^\lambda \\ %:tags: G0
        % Challenge bit (all games)
        b \getsr \{0,1\} \\
        % Run adversary with access to the LR encryption oracle (all games)
        b' \gets \Adversary^{\mathsf{LR}}() \\
        \pcreturn (b' = b)
    }

%%% -- Left-or-Right encryption oracle -------------------------------------------

    \procedure[linenumbering]{$\mathsf{LR}(m_0, m_1)$}{

        % Fresh nonce for every query (all games)
        r \getsr \{0,1\}^\lambda \\
        % G0: compute the PRF output using the secret key
        y \gets \mathrm{PRF}(k, r) \\ %:tags: G0
        % G1: evaluate a truly random function at r
        y \gets \RF(r) \\ %:tags: G1
        % G2: use independently sampled randomness
        y \getsr \{0,1\}^\lambda \\ %:tags: G2
        % Red1: query the external PRF oracle (no local key)
        y \gets \OPRF(r) \\ %:tags: Red1
        % G0, Red1, G1, G2: XOR with the chosen message
        c \gets y \oplus m_b \\ %:tags: G0-G2
        % G3: ciphertext is uniformly random, independent of m_b
        c \getsr \{0,1\}^\lambda \\ %:tags: G3
        \pcreturn (r, c)
    }

\end{pcvstack}
```
