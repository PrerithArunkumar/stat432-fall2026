---
id: w04-prerith2-selection-versus-prediction
title: "Why does correlation break Lasso's variable selection long before it breaks its prediction?"
author: "Prerith Arunkumar (prerith2)"
---

Writing out the stationarity conditions for the Lasso was the first thing this week that made the estimator feel less
like a black box. For the `glmnet` objective

$$
L(\beta) = \frac{1}{2n}\|y - X\beta\|_2^2 + \lambda\|\beta\|_1,
$$

subdifferentiating gives, at the solution $\hat\beta$,

$$
\frac{1}{n}x_j^\top(y - X\hat\beta) = \lambda s_j,
\qquad
s_j =
\begin{cases}
\operatorname{sign}(\hat\beta_j) & \hat\beta_j \neq 0,\\
\text{some value in } [-1,1] & \hat\beta_j = 0.
\end{cases}
$$

When $X^\top X/n = I$ this decouples completely and every coordinate is a soft-threshold,
$\hat\beta_j = \operatorname{sign}(\hat\beta_j^{\text{ols}})(|\hat\beta_j^{\text{ols}}| - \lambda)_+$. What I want to
understand is what the off-diagonal entries of $X^\top X/n$ actually do to this system, because they seem to damage two
different things at two different rates.

Concretely, take the equicorrelated design we keep coming back to: $X \sim N(0, \Sigma)$ with $\Sigma_{jj}=1$ and
$\Sigma_{jk}=\rho$ for $j \neq k$, with only a handful of nonzero $\beta_j$. As $\rho$ grows from $0.1$ toward $0.9$,
my understanding is that the probability of recovering the exact support collapses fairly quickly, while the test MSE
degrades much more gently. Three things I would like explained or corrected:

1. **Why the active set becomes a near-tie.** If $x_j$ and $x_k$ are strongly correlated and both carry signal, then
   the two equations $\frac{1}{n}x_j^\top r = \lambda s_j$ and $\frac{1}{n}x_k^\top r = \lambda s_k$ are almost the same
   equation, so a whole family of splits of the joint effect between $\hat\beta_j$ and $\hat\beta_k$ produces nearly the
   same residual $r$. Is it right to say that the $L_1$ penalty then breaks the tie essentially arbitrarily — whichever
   of the two happens to have slightly larger empirical correlation with $y$ in this sample enters first, and the other
   gets suppressed — so that support recovery is being decided by noise rather than by signal? And is that the reason
   the *selected set* is unstable across simulation runs even though the *fit* is not?

2. **Whether there is a sharp condition separating the two.** I have seen the irrepresentable condition referred to as
   the thing that governs whether Lasso can recover the true support, roughly
   $\|X_{S^c}^\top X_S (X_S^\top X_S)^{-1} \operatorname{sign}(\beta_S)\|_\infty < 1$ where $S$ is the true support. For
   equicorrelated $\Sigma$ with $s$ nonzero coefficients all of the same sign, this should reduce to a clean inequality
   in $\rho$ and $s$ alone. Is there a threshold in $\rho$ past which it simply fails, and does that threshold line up
   with where the empirical selection probability falls off a cliff? I am specifically trying to see whether prediction
   and selection are governed by genuinely different quantities, or whether prediction is just the more forgiving
   functional of the same estimate.

3. **Whether the elastic net fixes the mechanism or only the symptom.** Adding an $L_2$ term makes the objective
   strongly convex, so the near-tie in (1) should have a unique resolution instead of an arbitrary one. I have seen a
   grouping-effect bound of the form

   $$
   |\hat\beta_j - \hat\beta_k| \le \frac{C}{\lambda_2}\sqrt{2(1-\hat\rho_{jk})},
   $$

   which says correlated predictors are forced to receive similar coefficients. If that is the right statement, it seems
   to say the elastic net does not recover the true support either — it deliberately keeps the whole correlated block —
   it just fails in a stable, predictable direction instead of a random one. Is "stably wrong" actually the thing we
   want here, and is that why elastic net tends to win on prediction in correlated designs while still not being a
   support-recovery method?

The summary I am trying to get to is something like: the $L_1$ penalty is a good *prediction* device under correlation
and a fragile *selection* device, and those are not two views of one property but two genuinely different questions
about $\hat\beta_\lambda$. I would like to know whether that framing is right, and where it breaks down.
