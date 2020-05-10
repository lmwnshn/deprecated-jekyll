---
layout: page
title: Alpha-Beta Hack
permalink: /piazza/jklix7i05zj1mw_1037/
---

## Note

**Alpha Beta Lab Practice**

Hi all,

In lab today we handed out a blank tree for you to get some practice with alpha beta. The solutions to the alpha beta prune are here. There is a tree with the bounds written on it, and a chart with a step-by-step "trace" of how we calculate each bound.

If you have not filled out the tree by hand, please do so before starting to code. Simply reading the solutions to an alpha beta pruning is not enough to gain understanding of the algorithm.

The alpha-beta table is here, courtesy of Jeanne:

```
| Player |  v≤α   |  α<v<β   |  v≥β   |
|-------------------------------------|
| Minnie | Prune  | Update β | Ignore |
| Maxie  | Ignore | Update α | Prune  |
```

## Followup

The alpha-beta table is absolutely the right way to go for conceptual understanding, but a hack that produces the same results faster:

1. no take-backs

    - bounds are attached to edges
    - write down the bound as you go down the edge, and never modify the bound once you have it written down

2. just update

    - maxie always updates left bound as max(left, x)
    - minnie always updates right bound as min(right, x)
    - if you end up writing an invalid bound like (7,5) or in general (a,b) with a > b then you should have pruned
