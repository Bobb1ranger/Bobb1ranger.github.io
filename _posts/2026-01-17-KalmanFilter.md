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

Consider the discrete-time dynamical system:

$$
\begin{align*}
x_{k+1} & = A_k x_k + B_k u_k + w_k \\
z_k & = C_k x_k + v_k
\end{align*}
$$

where $$x_k$$ is the state vector at time step $$k$$, $$u_k$$ is the control input, $$z_k$$ is the measurement vector, and $$w_k$$ and $$v_k$$ are process and measurement noise, respectively. The noise terms are assumed to be zero-mean Gaussian with covariances $$Q_k$$ and $$R_k$$.

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
\hat{y}_{k} = z_k - C_k \hat{x}_{k|k-1} \\
S_k = C_k P_{k|k-1} C_k^T + R_k
\end{align*}
$$

Intuitively, the innovation represents the discrepancy between the actual measurement and the predicted measurement based on the a priori state estimate.

Kalman gain:

$$
K_k = P_{k|k-1} C_k^T S_k^{-1}
$$

The gain balances the trust between the a priori estimate and the new measurement. Intuitively:
*   If the measurement noise covariance $$R_k$$ is small (i.e., measurements are reliable), the Kalman gain will be higher, giving more weight to the measurement.
*   If the priori estimate covariance $$ {P}_{k \vert k-1}$$ is small (i.e., the model prediction is reliable), the Kalman gain will be lower, giving more weight to the model prediction.

A posteriori estimate (update):

$$
\begin{align*}
\hat{x}_{k|k} & = \hat{x}_{k|k-1} + K_k \hat{y}_k = (I - K_k C_k) \hat{x}_{k|k-1} + K_k z_k, \\
P_{k|k} & = (I - K_k C_k) P_{k|k-1} = (I - K_k C_k) P_{k|k-1} (I - K_k C_k)^T + K_k R_k K_k^T.
\end{align*}
$$

The posteriori covariance matrix $${P}_{k \vert k}$$ can also be given as:

$$
P_{k|k} = P_{k \vert k-1} - P_{k \vert k-1} C_k^T (C_k P_{k \vert k-1} C_k^T + R_k)^{-1} C_k P_{k \vert k-1},
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
<!-- 
# 4. ** Relation to Optimal State-Feedback Control **

Define the estimation error:

$$
e_k = x_k - \hat{x}_{k|k} 
$$

Consider a posteriori estimate law $$q_k$$:

$$
\hat{x}_{k|k}  = \hat{x}_{k|k - 1} + q_k
$$

Then

$$
e_k = x_k - \hat{x}_{k|k - 1} - q_k = A_{k-1} e_{k-1} + w_{k-1} - q_k
$$

What we measure is

$$
\hat{y}_k = z_k - C_k \hat{x}_{k|k-1} = C_k e_k + v_k
$$ -->