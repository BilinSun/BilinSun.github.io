---
layout: post
title: "Exposing bogus applications of proof of contradiction"
date: 2023-07-14
tags: [mathematics]
---

Exposure to intuitionistic logic has caused me to take a scrutinizing look at the proof of contradiciton technique. It's strikingly common for a proof that $1$ is prositive to use proof by contradiction. (Here)[https://www.natecation.com/one-greater-than-zero/] is an example, and we present a simplified and slightly corrected version of that proof below. Recall that, in addition to the ring axioms, we have the order axioms for integers: let $\mathbb Z^+\subset\mathbb Z$ be the subset uniquely determined by the properties:

$$\begin{aligned}
\text{Additive closure:}&&(\forany a,b\in\mathbb Z^+),\ a+b\in\mathbb Z^+\text.\\
\text{Multiplicative closure:}&&(\forany a,b\in\mathbb Z^+),\ ab\in\mathbb Z^+\text.\\
\text{Nontriviality}&&0\not\in\mathbb Z^+\\
\text{Trichotomy}&&(\forany a\in\mathbb Z)\text{ exactly one of the following holds:}\\
&&a\in\mathbb Z^+,\ a=0,\text{ or }{-a}\in\mathbb Z^+\text.
\end{aligned}$$

And to say the a number is positive is to say it is in $\mathbb Z^+$. Now, the ring axiom assures that $1\neq0$, so by trichotomy, either $1$ is positive, or it's not. Suppose for contradicion that $1$ is not positive, so $-1$ is positive. Using ring axioms, we have that $(-1)(-1)=1$, and since $-1$ is positive, multiplicative closure of $\mathbb Z^+$ ensures $(-1)(-1)=1$ is positive, contradicting the assumption. Hence, we must have that $1$ is positive.

There is no way to formalize this contradiction in intuitionistic logic. We essentially proved $\neg1\in\mathbb Z^+\rightarrow1\in\mathbb Z^+$, but in intuitionistic logic, this only (leads to)[https://en.wikipedia.org/w/index.php?title=Consequentia_mirabilis&oldid=1258644560#Intuitionistic_logic] $\neg\neg1\in\mathbb Z^+$, as opposed to $1\in\mathbb Z^+$. However, it would be surprising if we can't even prove that $1$ is positive in intuitionistic logic.

A careful check reveals that the contradiction setup is not necessary. In place of

> The ring axiom assures that $1\neq0$, so by trichotomy, either $1$ is positive, or it's not. Suppose for contradicion that $1$ is not positive, so $-1$ is positive.

We could have used the (disjunction property)[https://ncatlab.org/nlab/revision/intuitionistic+logic/24#DisjunctionProperty]

> The ring axiom assures that $1\neq0$, so by trichotomy, either $1$ is positive, or $-1$ is positive, in which case,

and stopped at

> ... multiplicative closure of $\mathbb Z^+$ ensures $(-1)(-1)=1$ is positive [anyway].

We are done! Note that our work to make the proof constructive, we got a **shorter** valid classic proof as well. It's even clearer to read: "by trichotomy, either $1$ is positive, or it's not" actually glosses over quite a few steps, whereas "by trichotomy, either $1$ is positive, or $-1$ is positive" is just the disjunction property, and such case work is so natural it wouldn't surprise a layperson.

What *is* surprising is that in one case, we ended up with 1 being positive and not positive at the same time, and does nothing about it. This does not invalidify our proof: we had two cases, and in both, we proved 1 is positive. However, I'd love to give a closer-up narrative of the paradox. It's true that we can conclude $\bot$ in the case. However, using this $\bot$ to lead us back to the other case only adds to the length of our proof.

TL;DR: You may replace proof by contradiction using the disjunction property, and be pleasantly surprised.