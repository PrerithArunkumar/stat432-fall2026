---
id: w03-prerith2-curvature-step-size
title: "Why does the same ridge estimator get harder to compute as lambda shrinks?"
author: "Prerith Arunkumar (prerith2)"
---

The ridge estimator has a closed form, $\hat{\beta}_\lambda = (X^\top X + \lambda I)^{-1} X^\top y$, but this week we also computed it with gradient descent on $L(\beta) = \|y - X\beta\|^2 + \lambda \|\beta\|^2$. I understand the two should agree. What I am stuck on is the relationship between the estimator and the algorithm used to reach it.

The Hessian of $L$ is $2(X^\top X + \lambda I)$, so its eigenvalues are $2(d_j^2 + \lambda)$ where $d_j$ are the singular values of $X$. Gradient descent with step size $\eta$ contracts the error along eigen-direction $j$ by a factor of $|1 - 2\eta(d_j^2 + \lambda)|$. For this to converge in every direction we need $\eta < 1/(d_{\max}^2 + \lambda)$, but the slowest direction then contracts at roughly $1 - (d_{\min}^2 + \lambda)/(d_{\max}^2 + \lambda)$ per step.

If that is right, it leads to three things I would like explained or corrected:

1. **The estimator does not change, but its difficulty does.** With correlated predictors $d_{\min}$ is near zero, so when $\lambda$ is small the condition number is huge and gradient descent crawls; when $\lambda$ is large it converges quickly. Is it accurate to say that the penalty regularizes the *problem* (conditions the Hessian) at the same time it regularizes the *estimator* (shrinks $\hat{\beta}$), and that these are two separate effects that happen to share the same $\lambda$?

2. **A stopped algorithm is a different estimator.** If I run gradient descent from $\beta_0 = 0$ with $\lambda = 0$ and stop after $t$ iterations, the iterate $\beta_t$ has not yet moved along the low-curvature directions. It looks like an implicitly shrunk estimate, similar to ridge. Is there a precise sense in which "$t$ steps of gradient descent" is an estimator with its own bias and variance, distinct from the OLS or ridge estimator it would converge to? If so, which $\lambda$ does a given $(\eta, t)$ correspond to, even approximately?

3. **Where step-size failures show up.** If I choose $\eta$ slightly too large, the divergence should appear first along the highest-curvature direction, $d_{\max}$. Would I see that as one coefficient combination (the top right-singular vector) oscillating with growing amplitude while the others still converge? A small worked example with two predictors of very different scale, one with and one without standardization, would make this concrete, since standardizing should change $d_{\max}/d_{\min}$ and therefore the usable range of $\eta$.

I want to be able to say clearly what is a property of $\hat{\beta}_\lambda$ (the target) and what is a property of the path gradient descent takes toward it, since so far those feel blurred together.
