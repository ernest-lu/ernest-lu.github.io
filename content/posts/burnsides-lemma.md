---
title: Burnside's Lemma 
date: 2024-05-06
math: true

---

DRAFT

Writing this because we touched on it briefly in Math 113, but I remember seeing this used in other problems as well.

Before starting, we should know what a group and a group action are.

A group is a set of things attached with an associative binary operation.
This operation acts on 2 elements in our set at a time and returns a result that is still in our group. Additionally, within our group, we must have the existence of identity and inverse elements. Formally: 


<!-- REWRITE THIS -->
Burnside's lemma gives us insights to count the number orbits of a group and the set of objects it acts on.


Burnside's lemma lies in the analysis of elements $x \in X$ fixed by $g \in G$. Specifically, we wish to count the total number of pairs $(x, g)$ such that $g \in G$ is fixed by $x \in X$. If we iterate the group element, we can express this quantity as $\sum_{g \in G}{X_g}$. On the other hand, if we iterate the set element, we can express this as $\sum_{x \in X}{G_x}$. Since these quantities express the same total, we have: $\sum_{g \in G}{X_g} = \sum_{x \in X}{G_x}$.

Then, considering a specific quantity $G_x$, we can see that this is a normal subgroup of $G$. This is because we can express $G_x$ as the kernel of the natural map $f_x: G \rightarrow X$, $f_x(g) = gx$. The kernel, or all the elements that map to the identity $f_x(g) = x$ is in fact all the elements $g \in G$ that do not change $x$, which is how we defined $G_x$ Therefore, we can consider the quotient group of $G / G_x$. Observe that the size of this quotient group is the orbit of $x$ itself. This is because every element in an orbit corresponds to a singular coset of this group. 

We then have $\sum_{g \in G}{X_g} = \sum_{x \in X}{G_x} = \sum_{x \in X} \frac{|G|}{|Gx|}$, where the term in the denominator is $|Gx|$ as in the size of the orbit of $x$ (not $G_x$ which denotes which elements in $G$ fix $x$). Since each element $x$ is part of a singular orbit, the summand $\sum_{x \in X} \frac{|G|}{|Gx|}$ can be rewritten as $|G|\sum_{\textrm{orbit}}\sum_{g \in \textrm{orbit}} \frac{1}{\textrm{orbit_size}} = |G|\sum_{\textrm{orbit}} 1 = |G| * \textrm{num_orbits}$. Now, the number of orbits itself has shown up as a term in 




Problems using this technique
- https://cses.fi/problemset/task/2209
- https://cses.fi/problemset/task/2210
- https://codeforces.com/problemset/problem/1954/F


Sources:
- Fraleigh Abstract Algebra textbook