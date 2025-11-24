---
layout: post
title: Lecture note - Inexact Fixed-Point Iterations for Min-Max Problems
date: 2025-10-24 15:30:00-0500
description: Keypoints from Stephen Wright's Talk at University of Minnesota
tags: math convex_optimization duality
categories: optimization-lectures
featured: false
related_posts: false
---

I really enjoyed this insightful talk and the speaker's style of giving lectures. 

Disclaimer:
This is a summary of Stephen Wright's talk at University of Minnesota on October 24, 2025. The summary was prepared by an attendee and may not capture all details accurately. For complete understanding, please refer to the original [work](https://arxiv.org/abs/2402.05071) or contact the [speaker](https://wrightstephen.github.io/sw_proj//). This note is still under work.

# Abstract: 

We consider constrained, L-smooth, nonconvex-nonconcave min-max problems either satisfying $$\rho$$ -cohypomonotonicity or admitting a solution to the $$\rho$$-weakly Minty Variational Inequality (MVI), where larger values of the parameter $$\rho$$ > 0 correspond to a greater degree of nonconvexity. Relevant problem classes include two player reinforcement learning and interaction-dominant min-max problems. We proposed inexact variants of Halpern and Krasnoselskii-Mann (KM) iterations and show that they can tolerate more nonconvexity than previously proved.  We also provide stochastic algorithms which can tolerate the same amount of nonconvexity. Complexity results are proved in both cases. (Our improvements come from harnessing the recently proposed "conic nonexpansiveness" property of operators.) Finally, we provide a refined analysis for inexact Halpern iteration and propose a stochastic KM iteration with a multilevel Monte Carlo estimator. This talk represents joint work with Ahmet Alacaoglu and Donghwan Kim.

# Introduction

Consider the problem 

$$
\begin{equation}\label{eq:minmaxprob}
\min_{u\in U} \max_{v \in V} f(u,v)
\end{equation}
$$

where $$U\subseteq \mathbb{R}^m$$ and $$V\subseteq \mathbb{R}^n$$ are closed convex sets admitting efficient projection operators and $$f:\mathbb{R}^m \times \mathbb{R}^n \mapsto \mathbb{R}$$ is a function such that $$\nabla_u f(u,v)$$ and $$\nabla_v f(u,v)$$ are Lipschitz continuous.
The general setting where $$f(u,v)$$ can be nonconvex-nonconcave is extremely relevant in machine learning, with applications in generative adversarial networks (GAN).

The following non-monotone inclusion (NMI) problem generalizes \eqref{eq:minmaxprob}.

$$
\text{Find }x^* \in \mathbb{R}^d \text{ such that } 0 \in F(x^*) + G(x^ *),
$$

where $$F$$ is possibly nonmonotone but $$L$$-Lipschitz and $$G: \mathbb{R}^d \rightrightarrows \mathbb{R}^d$$ is maximally monotone. Note that we recover Problem \eqref{eq:minmaxprob} by letting:

$$
x = \begin{bmatrix}
u \\ v
\end{bmatrix}, \ 
F(x) = \begin{bmatrix}
\nabla_u f(u,v) \\ -\nabla_v f(u,v)
\end{bmatrix} \text{ and }
G(x) = \begin{bmatrix}
\partial{\iota_U} \\ \partial{\iota_V}
\end{bmatrix},
$$

where $$\iota_U$$ is the indicator functionfor set $$U$$. $$G$$ can be understood as simply the subgradient of the convex constraint $$u \in U$$ and $$ v \in V$$. For example, in 1-d case $$U \doteq [0,1]$$.

$$
\iota_U(u) =
\begin{cases}
0, & u \in U \\
+\infty, & u \notin U\\
\end{cases}, \text{ and }
\partial{\iota_U(u)} =
\begin{cases}
\{0\}, & u \in (0,1) \\
(-\infty,0], & u = 0\\
[0,\infty), & u = 1\\
\emptyset, & u \notin U
\end{cases}.
$$

The main assumption made is that $$F + G$$ is $$\rho$$-cohypomonotone.

# Preliminaries

## MVI condition and Weak MVI:
The Minty Variational Inequality (MVI) condition states that there exists a solution $$x^*$$ such that

* Minmax is a special case of NMI
* Weak MVI holds for y =x*

## Resolvent:
Resolvent of an operator $$H: \mathbb{R}^d \rightrightarrows \mathbb{R}^d $$ is defined as $$J_H := (I + H)^{-1}$$.

## Non-expansive operators

Construct firmly non-expansive operator from non-expansive operator

---

# Algorithm

## (1) Restate as a fixed-point problem

$$ 0 \in (F +G)(x^*)$$ is equivalent to 

$$
\begin{align}
& x^* = J_{\eta(F + G)}(x^*) = (I + \eta (F + G))^{-1} (x^*) \\
\text{or } & x^* = (1- \alpha) x^* + \alpha J_{\eta(F + G)}(x^*)\\
\end{align}
$$

## (2) Outer Loop

Apply a fixed-point iteration (KM or Halpern) that allows for **inexact** evaluation of the resolvent.

* Inexactness: $$ \|\tilde{J}_{\eta(F + G)} (x_k) - J_{\eta(F + G)} (x_k)\|^2 \leq \epsilon_k^2$$. Choice of $$\epsilon_k$$ is crucial.
* Halpern: $$ x_{k+1} = \beta_k x_0 + (1-\beta_k)((1- \alpha) x_k + \alpha \tilde{J}_{\eta(F + G)}(x_k))$$
* KM: $$ x_{k+1} = \beta_k x_k + (1-\beta_k)\tilde{J}_{\eta(F + G)}(x_k)$$.

## (3) Inner Loop
Use an optimal first-order algorithm to calculate $$\tilde{J}_{\eta(F + G)}(x_k)$$, choosing $$\eta$$ to ensure that this subproblem is strongly convex- strongly concave. (Stochastic) extragradient can solve this problem with optimal first-order complexity.

### Details:
* Sublinear convergence rate when there is no exact gradient 
* FBF Forward-Backward-Forward Algorithm


Stochastic Access F via an unbiased oracle Fhat


Solve z \in (I _eta(F+G))(z) 
J \etaF = ( I + \eta F)^(-1) (u,v) =
Argmin max f(u’ , v’) + ½\eta (u’ - u)^2 - ½\eta(v’-v)^2
