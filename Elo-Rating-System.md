---
title: Elo Rating System
date: 2017-11-14 15:13:21
tags:
- Math
---

If Player $A$ has a rating of $R_{A}$ and Player $B$ a rating of $R_{B}$, the exact formula (using the logistic curve) for the expected score of Player $A$ is
$$
E_A = \frac{1}{1+10^{\frac{R_B-R_A}{400}}}
$$

Similarly the expected score for Player $B$ is

$$
E_B = \frac{1}{1+10^{\frac{R_A-R_B}{400}}}
$$

This could also be expressed by

$$
E_A = \frac{Q_A}{Q_A + Q_B}
$$

$$
E_B = \frac{Q_B}{Q_A + Q_B}
$$

where $Q_A = 10^{\tfrac{R_A}{400}}$ and $Q_B = 10^{\tfrac{R_B}{400}}$

Supposing Player $A$ was expected to score $E_{A}$ points but actually scored $S_{A}$ points. The formula for updating their rating is
$$
R_{A}^{\prime }=R_{A}+K(S_{A}-E_{A})
$$
