---
layout: post
title: Normed Vector Space
date: 2025-04-05 01:41:00-0400
description: a quick note from David Luenberger's book "Optimization by Vector Space Methods"
tags: Math, Vector Space, Banach Space, Convex Optimization, Duality,
categories: sample-posts
related_posts: false
---

Given a complex-valued $$ m \times n$$ matrix $$A$$, $$A^H$$ denotes its hermitian conjugate.
$$\ell_2^{m\times n}[Z]$$ denotes the Hilbert space of sequences of $$m\times n$$ complex-valued matrices, with inner product defined as

$$
\langle H,G\rangle = \sum_{k=-\infty}^{\infty} \mathrm{trace}(G^H[k]H[k]).
$$

The $$\ell_2^{m\times n}[Z]$$ space can be decomposed as the direct sum of two spaces of sequences
$$
\ell_2^{m\times n}[Z^+] \oplus \ell_2^{m\times n}[Z^-]
$$
. The unilateral z-transform of $$G\in \ell_2^{m\times n}[Z+]$$ is
$$
\hat{G}(z) = \sum_{k=0}^\infty G[k] z^{-k}
$$

$${\mathcal{H}_2^{m \times n}}^\perp$$ is the set of functions $$\hat{G} : \mathbb{C} \to \mathbb{C}^{m \times n}$$ such that


$$
\frac{1}{2\pi j} \oint_{\mathcal{C}} \hat{G}(z) z^{n-1} \, dz = \mathbf{0}, \quad \forall n \geq 0
$$

There is a unique decomposition into $$\mathcal{H}_2$$ and $$\mathcal{H}_2^\perp$$ for all matrix-valued functions analytic on the unit circle, by the Projection Theorem.

$$\mathcal{RH}_2$$ and $$\mathcal{RH}_2^\perp$$ respectively denote the rational proper transfer function matrices in $$\mathcal{H}_2$$ and $$\mathcal{H}_2^\perp$$.


