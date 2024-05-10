---
title: Burnside's Lemma 
date: 2024-05-06
math: true

---

DRAFT

Writing this as semi-review for Math 113 final, even though this was only touched briefly. Has applications to competitive programming problems too. When I first encountered this idea, it seemed like I was applying some black magic formula to get a count for things. I felt like I was taking in unrelated quantities and outputting out something that could get my answer.

<!-- As a small intro to the power of using this lemma, consider the problem of counting how many ways there are to arrange $1..n$ on a circle, where rotations of the circle are considered the same. I.E for $n = 3$: in clockwise order $123$, $312$ are considered the same -->


Before starting, we should know what a group and a group action are.

A group is a set of things attached with an associative binary operation.
This operation acts on 2 elements in our set at a time and returns a result that is still in our group. Additionally, within our group, we must have the existence of identity and inverse elements. Formally: 

A group $G = (G, .)$ is a non-empty set furnished with a binary operation .
(usually omitted) with the following properties.
- the associative law: $(ab)c = a(bc) (a, b, c \in G)$ ;
- the existence of an identity element: there exists $e \in G$ such that, for all $a \in G$, $ea = a$ ;
- the existence of inverses: for all $a \in G$ there exists $a^{-1} \in G$ such that $a^{-1}a = e$ 

A group may act on a set, meaning that given a specific set $S$, construct a mapping from $f: G \times S \rightarrow S$ that associates pairs of (group element, set element) to set elements. Effectively, each group element can take a set element to another (possibly the same) set element. One example would be the group of $2 \times 2$ real-valued invertible matrices acting on a set of vectors.

Now, when we consider a specific element of the set $s \in S$, there is a subset of elements which are reachable from this $s$ using the group action. We call this the orbit of $s$ and define it as the set $\{ gs \} \forall g \in G$, where $gs$ is the result of the group action. Now, since we enforce that $G$ is a group, if $x$ is in the orbit of $y$, then $y$ is also in the orbit of $x$ because we can invert the group element that takes $x$ to $y$. Therefore, these orbits partition elements of the set into certain orbit classes, with each element in the set $S$ being a part of exactly one orbit.

<!-- REWRITE THIS -->
Burnside's lemma gives us insights to count the number orbits of a group and the set of objects it acts on. This is a powerful number to consider. A lot of times, we don't care about things that are equivalent under group operations (ie rotations of an object). Burnside's lemma gives us a way to count the number of distinct objects discounting for group equivalence.

Burnside's lemma lies in the analysis of elements $x \in X$ fixed by $g \in G$. Specifically, it mainly considers the total number of pairs $(x, g)$ such that $g \in G$ is fixed by $x \in X$. If we iterate the group element, we can express this quantity as $\sum_{g \in G}{|X_g|}$. On the other hand, if we iterate the set element, we can express this as $\sum_{x \in X}{|G_x|}$. Since these quantities express the same total, we have: $\sum_{g \in G}{|X_g|} = \sum_{x \in X}{|G_x|}$.

Then, considering a specific quantity $G_x$, we can see that this is a normal subgroup of $G$. This is because we can express $G_x$ as the kernel of the natural map $f_x: G \rightarrow X$, $f_x(g) = gx$. The kernel, or all the elements that map to the identity $f_x(g) = x$ is in fact all the elements $g \in G$ that do not change $x$, which is how we defined $G_x$. Therefore, we can consider the quotient group of $G / G_x$. Observe that the size of this quotient group is the size of the orbit of $x$ itself. This is because every element in an orbit corresponds to a singular coset of this group. By Lagrange's theorem, we can see that the number of cosets is $\textrm{num orbits} = |Gx| = |G / G_x| = \frac{|G|}{|G_x|}$. From this, we can see that $|G_x| = \frac{|G|}{|Gx|}$

If you haven't encountered Lagrange's theorem before, it just asserts that the size of every coset in a quotient group is equal by constructing a bijection between any two cosets. Using this fact, and the fact that every element of the group is partitioned into a singular coset, we get that the number of cosets is just $|G|$ divided by the size of a singular coset. Additionally, since the size of the orbit equals the size of the quotient group, we have: 


We then have $\sum_{g \in G}{X_g} = \sum_{x \in X}{G_x} = \sum_{x \in X} \frac{|G|}{|Gx|}$, where the term in the denominator is $|Gx|$ as in the size of the orbit of $x$ (not $G_x$ which denotes which elements in $G$ fix $x$). Since each element $x$ is part of a singular orbit, the summand $\sum_{x \in X} \frac{|G|}{|Gx|}$ can be rewritten as $|G|\sum_{\textrm{orbit}}\sum_{g \in \textrm{orbit}} \frac{1}{\textrm{orbit size}} = |G|\sum_{\textrm{orbit}} 1 = |G| * \textrm{num orbits}$. Now, the number of orbits itself has shown up as a term in our summand. 

The way the number of orbits is why I thought of this as almost black magic. Lagrange's theorem enabled us to derive a fractional quantity with denominator being the orbit size for every single element. The sum of these all together is equal to the number of orbits because we can think of each element as contributing their own part to the number of orbits.




Problems using this technique
- https://cses.fi/problemset/task/2209
- https://cses.fi/problemset/task/2210
- https://codeforces.com/problemset/problem/1954/F


Sources:
- Fraleigh Abstract Algebra textbook