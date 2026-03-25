---
layout: post
title: Kalman Filter Recap
date: 2026-01-19 10:44:00+0000
description: A recap of the Kalman Filter algorithm for state estimation. It also covers how it is related to the optimal state-feedback control problem.
tags: control math optimization
related_posts: false
authors:
    - name: Yuanji Zou
      url: "https://bobb1ranger.github.io/"
---

---

# 1. **Math Formulation of the Kalman Filter**

The following table summarizes the notations:

**Table 1. Kalman filter state estimation notation** 

| Notation             | Name / Term              | Meaning                                                                  |
|----------------------|--------------------------|--------------------------------------------------------------------------|
| $$x_k$$              | True state               | The actual (unknown) system state at time step $$k$$                          |
| $$\hat{x}_{k\vert k-1}$$  | A priori estimate        | Estimate of $$x_k$$ using observations up to time $$k-1$$                 |
| $$\hat{x}_{k\vert k}$$    | A posteriori estimate    | Updated estimate of $$x_k$$ after incorporating measurement at time $$k$$ |
| $$e_{k\vert k-1}$$        | A priori error           | $$x_k - \hat{x}_{k\vert k-1}$$ (prediction error before seeing $$z_k$$)   |
| $$e_{k\vert k}$$   | A posteriori error       | $$x_k - \hat{x}_{k\vert k}$$ (estimation error after update)                     |
| $$P_{k\vert k-1}$$ | A priori covariance      | $$\mathbb{E}[e_{k\vert k-1} e_{k\vert k-1}^T]$$ (uncertainty before measurement) |
| $$P_{k\vert k}$$   | A posteriori covariance  | $$\mathbb{E}[e_{k \vert k} e_{k\vert k}^T]$$ (uncertainty after measurement)     |


Consider the discrete-time dynamical system:

$$
\begin{align*}
x_{k+1} & = A_k x_k + B_k u_k + w_k \\
z_k & = C_k x_k + v_k
\end{align*}
$$

where $$x_k$$ is the state vector at time step $$k$$, $$u_k$$ is the control input, $$z_k$$ is the measurement vector, and $$w_k$$ and $$v_k$$ are process and measurement noise, respectively. The noise terms $$ w_k$$ and $$ v_k $$ are assumed to be zero-mean Gaussian with covariances $$Q_k$$ and $$R_k$$.

$$
\begin{equation*}
\mathrm{cov} [w_k] = Q_k;\quad \mathrm{cov} [v_k] = R_k.
\end{equation*}
$$

A priori estimate (prediction):

$$
\begin{align*}
\hat{x}_{k|k-1} & = A_{k-1} \hat{x}_{k-1|k-1} + B_{k-1} u_{k-1} \\
P_{k|k-1} & = A_{k-1} P_{k-1|k-1} A_{k-1}^T + Q_{k-1}
\end{align*} 
$$

The innovation (prefit residual) and its covariance are given by:

$$
\begin{align*}
\hat{y}_{k} & = z_k - C_k \hat{x}_{k|k-1} \\
S_k & \doteq \mathrm{cov}[\hat{y}_k] = C_k P_{k|k-1} C_k^T + R_k
\end{align*}
$$

Intuitively, the innovation represents the discrepancy between the actual measurement and the predicted measurement based on the a priori state estimate.

The Kalman gain used to update the posteriori estimate is given by $$~\eqref{eqn:KalmanGain}$$:

$$
\begin{equation}\label{eqn:KalmanGain}
K_k = P_{k|k-1} C_k^T S_k^{-1}
\end{equation}
$$

A posteriori estimate (update):

$$
\begin{align*}
\hat{x}_{k\vert k} & = \hat{x}_{k\vert k-1} + K_k \hat{y}_k = (I - K_k C_k) \hat{x}_{k\vert k-1} + K_k z_k, \\
P_{k\vert k} & = (I - K_k C_k) P_{k\vert k-1} = (I - K_k C_k) P_{k\vert k-1} (I - K_k C_k)^T + K_k R_k K_k^T.
\end{align*}
$$

The gain balances the trust between the a priori estimate and the new measurement. Intuitively:
*   If the measurement noise covariance $$R_k$$ is small (i.e., measurements are reliable), the Kalman gain will be higher, giving more weight to the measurement.
*   If the priori estimate covariance $$ {P}_{k \vert k-1}$$ is small (i.e., the model prediction is reliable), the Kalman gain will be lower, giving more weight to the model prediction.

The posteriori covariance matrix $${P}_{k \vert k}$$ can also be given as a more symmetric form:

$$
\begin{equation*}
P_{k\vert k} = P_{k \vert k-1} - P_{k \vert k-1} C_k^T (C_k P_{k \vert k-1} C_k^T + R_k)^{-1} C_k P_{k \vert k-1},
\end{equation*}
$$

which leads to the priori covariance update as a discrete-time Riccati equation:

$$
\begin{equation}\label{eq:riccati}
P_{k+1|k} = A_k P_{k \vert k-1} A_k^T + Q_k - A_k P_{k \vert k-1} C_k^T (C_k P_{k \vert k-1} C_k^T + R_k)^{-1} C_k P_{k \vert k-1} A_k^T.
\end{equation}
$$

This Riccati equation is independent of the control input matrix $$B_k$$, since the estimation error dynamics do not depend on the control inputs in linear systems.
(When the system if time-invariant, the steady-state solution can be found by solving the algebraic Riccati equation.)

---

# 2. **Optimality of the Kalman Filter**

The optimality of the Kalman filter is very strong, but also conditional.

## 2.1 Optimal in the MMSE sense:
The Kalman filter produces the estimate $$\hat{x}_{k|k}$$ that minimizes the mean squared error (MMSE) between the true state $$x_k$$ and the estimate $$\hat{x}_{k|k}$$, given all measurements up to time step $$k$$. 

$$
\begin{align*}
\hat{x}_{k|k} &= \mathbb{E}[x_k | z_1, z_2, \ldots, z_k] \\
\hat{x}_{k|k} &= \arg\min_{\tilde{x}} \mathbb{E} \left[ \| x_k - \tilde{x} \|^2 \mid z_1, z_2, \ldots, z_k \right]
\end{align*}
$$

When the system involves multivariate states and measurements, the Kalman filter minimizes the trace of the error covariance matrix:

$$
\hat{x}_{k|k} = \arg\min_{\tilde{x}} \text{trace} \left( \mathbb{E} \left[ (x_k - \tilde{x})(x_k - \tilde{x})^T \mid z_1, z_2, \ldots, z_k \right] \right) = \arg\min_{\tilde{x}} \text{trace}(P_{k|k})
$$

This optimality holds under the assumptions of linearity and Gaussian noise.
*   The system is linear.
*   $$w_k$$ and $$v_k$$ are zero-mean Gaussian noise with known covariances.
*   The initial state $$x_0$$ is Gaussian with known mean and covariance.

## 2.2 Exact Bayesian filter for linear–Gaussian systems:
Under these assumptions, one can show that the Kalman filter is equivalent to the optimal Bayesian filter for linear–Gaussian systems. The posterior distribution of the state given the measurements is Gaussian, and the Kalman filter provides the exact mean and covariance of this distribution.
*  The posterior distribution $${p}(x_k \vert z_1, z_2, \ldots, z_k)$$ is Gaussian.
*  The Kalman filter computes the mean and covariance of this posterior distribution exactly.
*  No approximation is involved.

## 2.3 Best Linear Unbiased Estimator:

Even without Gaussianity, the Kalman filter is still optimal among all linear unbiased estimators. KF minimmizes the estimation error covariance among all linear estimators that are unbiased. This is the given by the Gauss–Markov theorem.

---

# 3. **Extended Kalman Filter (EKF)**

For nonlinear systems, the Extended Kalman Filter (EKF) linearizes the system around the current estimate to apply the Kalman filter framework. The EKF approximates the state transition and measurement functions using their Jacobians.

$$
\begin{align*}
x_k & = f(x_{k-1}, u_{k-1}) + w_{k-1} \\
z_k & = h(x_k) + v_k
\end{align*}
$$

Predict:

$$
\begin{align*}
\hat{x}_{k|k-1} & = f(\hat{x}_{k-1|k-1}, u_{k-1}) \\
P_{k|k-1} & = F_{k-1} P_{k-1|k-1} F_{k-1}^T + Q_{k-1};\quad  F_{k-1} = \left. \frac{\partial f}{\partial x} \right|_{x = \hat{x}_{k-1|k-1}, u = u_{k-1}}
\end{align*}
$$

Innovate:

$$
\begin{align*}
\hat{y}_k & = z_k - h(\hat{x}_{k|k-1}) \\
S_k & = H_k P_{k|k-1} H_k^T + R_k;\quad H_k = \left. \frac{\partial h}{\partial x} \right|_{x = \hat{x}_{k|k-1}}
\end{align*}
$$

Kalman Gain:

$$
K_k = P_{k|k-1} H_k^T S_k^{-1}
$$

Update:

$$
\begin{align*}
\hat{x}_{k|k} & = \hat{x}_{k|k-1} + K_k \hat{y}_k \\
P_{k|k} & = (I - K_k H_k) P_{k|k-1}
\end{align*}
$$

There is no optimality guarantee for EKF in general, but it works well in practice for many applications. For slowly varying nonlinear systems, EKF can provide good approximations to the optimal Bayesian filter.

---
 
# 4. **Relation to Optimal State-Feedback Control**

## 4.1 Equivalent LQG formulation

A posterior estimation error and priori estimation error:

$$
\begin{align*}
e_{k\vert k} & = x_k - \hat{x}_{k\vert k} \\
e_{k\vert k - 1} & = x_k - \hat{x}_{k\vert k-1}
\end{align*}
$$

The innovation at step $$k$$ is just their difference

$$
\begin{equation}
\nu_k = e_{k\vert k-1} - e_{k\vert k}
\end{equation}
$$

We initialize the A priori estimate at step $$0$$ to be $$\hat{x}_{0\vert -1} = \mathbf{0}$$, which gives $$e_{k\vert k - 1} = x_0$$.
Rewrite the system dynamics difference equation to formulate a LQG-like problem, assuming $$A$$, $$C$$ matrices are time-invariant:

$$
\begin{align*}
e_{k + 1\vert k} & = A e_{k\vert k - 1} + w_k - A \nu_k \\
e_{k\vert k} & = e_{k\vert k - 1} - \nu_k \\
y_k & = C e_{k\vert k - 1} + v_k \\
\end{align*}
$$

At each time-step, to optimally estimate $$x_k$$, we need to optimize by measuring the noisy projection of $$e_{k\vert k - 1}$$ its history:

$$
J_k = \mathrm{trace} \left( \mathbb{E} \left[ e_{k\vert k} e_{k\vert k}^T \mid y_0, y_1, y_2, \ldots, y_k \right] \right) 
$$

Only the terminal state is involved in the cost function, which means we don't care about the intermediate steps. Those intermediate estimates and covariances are just computational artifacts.
*  You can compute everything at time $$k$$ in one shot(To be extreme) or compute it recursively step-by-step (Kalman Filtering)
*  The result is identical by either strategy.

This is generally the nature of estimation task - **Later actions can always correct previous non-optimality and there are infinite-many combinations of $$\{\mu_0,..., \mu_k\}$$ to achieve the optimal terminal estimation accuracy given a fixed horizon $$k$$**. 
In other words, we don't need to compromise for the future.
**It's allowable to be greedy at each step and iteratively obtain the optimal terminal estimate.** We can modify the cost into an sum over the horizon without affecting the terminal optimality. 

$$
\begin{equation}
\begin{split}
J_k = & \min \sum_{j = 1}^{k} \mathrm{trace} \left( \mathbb{E} \left[ e_{j\vert j} e_{j\vert j}^T \mid y_0, y_1, y_2, \ldots, y_j \right] \right) \\
= & J_{k - 1} + \min \mathrm{trace} \left( \mathbb{E} \left[e_{k\vert k} e_{k\vert k}^T \mid y_0, y_1, y_2, \ldots, y_k \right] \right)
\end{split}
\end{equation}
$$

Minimizing this sum will ensure the optimal estimates at all steps.

## 4.2 Constraints on the input sequence

In general，$$\mu_k = f(y_0, y_1, y_2, \ldots, y_k)$$. We hypothize that it's a linear combination of observations $$y_0, ..., y_k$$.

$$
\begin{equation}
\mu_k = L_0 y_0 + ... + L_k y_k = L Y_k
\end{equation}
$$

The primal optimization problem is

$$
\begin{align}
J(L) = & \min_{L}  \mathrm{trace} \left(\mathbb{E}\left[e_{k\vert k}e_{k\vert k}^\top\right]\right) \\
\mathbb{E}\left[e_{k\vert k}e_{k\vert k}^\top\right] = & \mathbb{E}\left[(e_{k\vert k - 1} - L Y_k) (e_{k\vert k - 1} - L Y_k)^\top\right] \\
= & \mathbb{E}\left[e_{k\vert k -1}e_{k\vert k-1}^\top\right] - \mathbb{E}\left[e_{k\vert k - 1}Y_k^\top\right] L^\top - L \mathbb{E}\left[Y_k e_{k\vert k - 1}^\top\right] + L \mathbb{E}\left[Y_k Y_k^\top\right] L^\top \\
\end{align}
$$

Define $$P_{eY} = \mathbb{E}\left[e_{k\vert k - 1}Y_k^\top\right]$$ and $$P_Y = \mathbb{E}\left[Y_k Y_k^\top\right]$$. Then

$$
\begin{equation*}
\mathbb{E}\left[e_{k\vert k}e_{k\vert k}^\top\right] = P_x - P_{eY} L^\top - L P_{eY}^\top + L P_Y L^\top.
\end{equation*}
$$

By taking derivatives over $$L$$, we find that the optimal $$L^\circ = $P_{eY} P_Y^{-1}$$. By such selection of $$L$$,

$$
\begin{equation}
\mathbb{E}\left[e_{k\vert k}Y_k^\top\right] = \mathbb{E}\left[(e_{k\vert k - 1} - L Y_k)Y_k^\top\right] = \mathbf{0}.
\end{equation}
$$

This suggests that each A posteriori estimate has an error orthogonal to all the history observations by expectation.

