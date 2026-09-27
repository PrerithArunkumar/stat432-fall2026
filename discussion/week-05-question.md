---
id: w05-prerith2-why-larger-k-adds-bias
title: "Why does averaging more neighbors make KNN more biased instead of more accurate?"
author: prerith2
---

I am confused about why increasing $k$ increases the bias of KNN. My intuition was that averaging more responses
should give a better estimate, because the noise averages out. That part does show up in the variance: in my
Homework 05 simulation at $\mathbf x_0 = (0.5, 0.7, 1)^{\mathsf T}$ with $n = 400$, the variance fell from about
$0.042$ at $k = 1$ to about $0.006$ at $k = 29$. But the squared bias rose from about $0.0003$ to $0.015$, and the
mean squared error was smallest around $k = 9$.

1. Is the bias growing because the $k$ nearest neighbors spread farther from $\mathbf x_0$, so we end up averaging
   $f$ at points where it differs from $f(\mathbf x_0)$?
2. If so, what decides how fast the bias grows with $k$: the shape of $f$ near $\mathbf x_0$, how densely the
   training points cover that region, or both?
