---
layout: post
title: Time-domain game interpretation of $$H_\infty$$ control
date: 2025-11-10 21:06:00+0000
description: A simple, time-domain derivation from the game perspective provides an intuitive understanding of where the $$H_\infty$$ Riccati equation comes from. It's a direct application of dynamic programming, similar to the LQR derivation.
tags: Control, Math, Optimization.
related_posts: false
---

# 1. The "Game" Setup

We define the **system**, the **players**, and the **rules**.

## System Dynamics

$$
x_{k+1} = A x_k + B_1 w_k + B_2 u_k.
$$

We use a standard LQR-like output for simplicity so that $$ \|z_k\|_2^2 = x_k^T Q x_k + u_k^T R u_k $$:

$$
z_k = C_1 x_k + D_{12} u_k =
\begin{bmatrix}
Q^{1/2} x_k \\
R^{1/2} u_k
\end{bmatrix}
$$

## Players
* **Controller** ($$u_k$$): tries to minimize the cost.  
* **Disturbance** ($$w_k$$): tries to maximize the cost.

## The Game's Objective
The $$H_\infty $$ goal is to ensure the energy gain is bounded: $$ \sum_{k=0}^{\infty} \|z_k\|_2^2 < \gamma^2 \sum_{k=0}^{\infty} \|w_k\|_2^2 $$. Define:

$$
J = \sum_{k=0}^{\infty} \left( \|z_k\|_2^2 - \gamma^2 \|w_k\|_2^2 \right)
$$

This can be rewritten as a **zero-sum game**:
- The controller $$u_k$$ tries to **minimize** the score.
- The disturbance $$w_k$$ tries to **maximize** it.

---

# 2. The "Scorecard" (Dynamic Programming)

We apply **dynamic programming** to find the "value" of the game.

## Value Function
Define the **cost-to-go**:

$$
V(x_k) = x_k^T P x_k
$$

## Bellman Equation

$$
V(x_k) = \min_{u_k} \max_{w_k} 
\left\{
\|z_k\|_2^2 - \gamma^2 \|w_k\|_2^2 + V(x_{k+1})
\right\}
$$

Substitute the known quantities:
<span style="color:red">Why the game cost is formulated as a min-max rather than a max-min program?</span>

When analyzing the discrete-time or continuous-time $$H_\infty$$ control problem, the dynamic programming approach leads to a **min–max optimization**:
the controller minimizes cost while the disturbance maximizes it.  
A natural question is: *what happens if we flip the order to max–min?*

### 1. Meaning of Changing the Order

1. **Standard formulation (min–max):**

   The controller picks $$u_k$$ to minimize the worst-case effect of $$w_k$$. This corresponds to the full information control:

   $$
   V(x_k) = \min_{u_k} \max_{w_k} \left\{ \ell(x_k,u_k,w_k) + V(x_{k+1}) \right\}.
   $$

2. **Flipped formulation (max–min):**

   The controller must decide first (announce $$u_k$$ or even a strategy), and the disturbance reacts after observing it. This means the controller don't know about the disturbance at current step:

   $$
   V(x_k) = \max_{w_k} \min_{u_k} \left\{ \ell(x_k,u_k,w_k) + V(x_{k+1}) \right\}.
   $$

If the cost function is **convex in $$u$$** and **concave in $$w$$**, then the two formulations are equivalent due to the *minimax theorem* (saddle point exists). Otherwise, the order matters — typically, the controller is worse off when forced to move first (a Stackelberg-type disadvantage).

### 2. Quadratic Stage Cost

For the standard \(H_\infty\) setting, define

$$
L(u,w)
= x^\top Q x + u^\top R u - \gamma^2 w^\top w
+ (A x + B_1 w + B_2 u)^\top P (A x + B_1 w + B_2 u).
$$

Here \(P\) is the value function matrix at the next step in the dynamic programming recursion.

### 3. Convex–Concave (Isaacs) Condition

Compute the Hessians with respect to $$u$$ and $$w$$:

$$
\frac{\partial^2 L}{\partial u^2} = 2(R + B_2^\top P B_2),
\qquad
\frac{\partial^2 L}{\partial w^2} = -2(\gamma^2 I - B_1^\top P B_1).
$$

Thus:

- $$L$$ is **convex in $$u$$** if $$R + B_2^\top P B_2 \succ 0$$.
- $$L$$ is concave in $$w$$ if $$ \gamma^2 I - B_1^\top P B_1 \succ 0$$.

This last inequality is the **Isaacs condition** (or the saddle-point condition). When it holds, we can swap the order of optimization:

$$
\min_u \max_w L(u,w) = \max_w \min_u L(u,w),
$$

and the $$H_\infty$$ Riccati equation derived via dynamic programming remains valid.

### 4. Intuitive Interpretation

- If the Isaacs condition holds, the disturbance’s energy is bounded by $$\gamma$$, ensuring that the problem has a well-defined saddle point.  
  The order of min–max does not matter.

- If the condition fails, $$L$$ is no longer concave in $$w$$; the disturbance can exploit the controller’s pre-commitment, and  
  the **max–min value** (controller as leader) becomes larger — meaning worse performance for the controller.

This is analogous to the **Stackelberg vs. Nash** distinction in game theory.

### 5. Practical Check

After solving the Riccati or LMI for a given $$\gamma$$, test:

$$
M = \gamma^2 I - B_1^\top P B_1.
$$

If $$M \succ 0$$, then the Isaacs condition holds and  
$$\min\max = \max\min$$.  
If $$M$$ has nonpositive eigenvalues, the saddle-point equivalence fails.

$$
x_k^T P x_k = 
\min_{u_k} \max_{w_k}
\left\{
x_k^T Q x_k + u_k^T R u_k - \gamma^2 w_k^T w_k
+ x_{k+1}^T P x_{k+1}
\right\}
$$

---

# 3. Solving the Game (finding the moves)

We denote the expression inside as $$L$$:

$$
L = x_k^T Q x_k + u_k^T R u_k - \gamma^2 w_k^T w_k + 
(A x_k + B_1 w_k + B_2 u_k)^T P (A x_k + B_1 w_k + B_2 u_k)
$$

## Step 1: The Disturbance’s Move (maximize $$L$$)

$$
\frac{\partial L}{\partial w_k} = -2\gamma^2 w_k + 2 B_1^T P (A x_k + B_1 w_k + B_2 u_k) = 0
$$

Solve for $$ w_k^* $$:
$$
w_k^* = (\gamma^2 I - B_1^T P B_1)^{-1} B_1^T P (A x_k + B_2 u_k)
$$

**Key insight:**  
This is the **worst-case disturbance**, dependent on both $$x_k$$ and $$u_k$$.

---

## Step 2: The Controller’s Move (minimize $$L$$)
$$
\frac{\partial L}{\partial u_k} = 2R u_k + 2 B_2^T P (A x_k + B_1 w_k + B_2 u_k) = 0
$$
Solve for $$ u_k^* $$:
$$
u_k^* = -(R + B_2^T P B_2)^{-1} B_2^T P (A x_k + B_1 w_k)
$$

**Problem:** $$u_k^*$$ depends on $$w_k$$, which is not known in real time.  
**Solution:** Assume the controller uses state feedback: $$u_k = -K x_k$$ and the disturbance knows this strategy. (This is equivalent to the max-min formulation.)
* Note: we still don't know how this assumption should be changed when $$u_k$$ must subject to some strict causality on $$x_k$$.

---

# 4. The Result: The $$ H_\infty$$ Riccati Equation

We plug the worst-case disturbance $$w_k^*$$ into $$L$$, then find the $$u_k$$ minimizing the result.  
After algebraic simplification (as in Doyle et al.), the Bellman equation yields the **Discrete-Time Algebraic Riccati Equation (DARE)** for $$H_\infty$$ control:

$$
P = Q + A^T P A - 
A^T P 
\begin{bmatrix} B_1 & B_2 \end{bmatrix}
\left(
R_{H_\infty} +
\begin{bmatrix} B_1 & B_2 \end{bmatrix}^T 
P 
\begin{bmatrix} B_1 & B_2 \end{bmatrix}
\right)^{-1}
\begin{bmatrix} B_1 & B_2 \end{bmatrix}^T
P A
$$

where
$$
R_{H_\infty} = 
\begin{bmatrix}
-\gamma^2 I & 0 \\
0 & R
\end{bmatrix}.
$$

---

# 5. LQR vs. $$ H_\infty $$ (a simple comparison)

| Control Type | Riccati Equation | Interpretation |
|:-------------:|:----------------:|:---------------:|
| **LQR (H₂)** <br><br> | <br>$$ P = Q + A^T P A - A^T P B_2 (R + B_2^T P B_2)^{-1} B_2^T P A $$<br><br> | <br>Controller minimizes its own cost<br><br> |
| **$$H_\infty$$** <br><br> | <br>$$ P = Q + A^T P A - A^T P B_{\text{all}} R_{\text{all}}^{-1} B_{\text{all}}^T P A $$<br><br> | <br>Controller competes with disturbance \(B_1 w\)<br><br> |

Here, $$B_{\text{all}}$$ and $$R_{\text{all}}$$ combine the control and disturbance effects:

$$
B_{\text{all}} = [B_1 \; B_2], \quad
R_{\text{all}} = 
\begin{bmatrix}
-\gamma^2 I & 0 \\
0 & R
\end{bmatrix}
$$

If  there is $$Y$$ and $$ X\succ 0$$ such that the following LMI holds:

$$
\begin{bmatrix}
X & (A X + B_2 Y)^{\top} & B_1^{\top} & (C_1 X + D_{12} Y)^{\top} \\
A X + B_2 Y & X & 0 & 0 \\
B_1 & 0 & \gamma^2 I & 0 \\
C_1 X + D_{12} Y & 0 & 0 & I
\end{bmatrix} \succ 0,
$$

then the following state-feedback control policy $$K = Y X^{-1}$$ ensures that $$J < 0$$ for all $$w \in \ell_2$$, i.e. $$\|T_{zw}\|_\infty< \gamma$$.

---

## Interpretation

* The $$ -\gamma^2 I $$ term acts as a **negative cost**, representing the **disturbance player's gain**.  
* The controller must design feedback strong enough to **counteract** it, ensuring that the closed-loop system remains stable.
* The $$ H_\infty$$ Riccati equation is just the LQR Riccati equation modified to include the **negative cost** of the maximizing disturbance player, scaled by $$ \frac{1}{\gamma^2} $$.

---

