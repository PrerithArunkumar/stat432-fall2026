---
id: w10-prerith2-forest-kernel-adaptive-neighborhoods
title: "Why would a forest kernel's neighborhood stretch along irrelevant predictors and shrink along relevant ones, and when is that adaptiveness a weakness?"
author: prerith2
---

A random forest’s prediction at a point x can be written as a weighted average of the training responses. Each training point gets a weight equal to how often, across trees, it falls in the same leaf as x, divided by that leaf’s size. In this sense the forest acts like a kernel or nearest-neighbor smoother, except that the “neighborhood” is learned from the data rather than fixed in advance.

Compare this forest kernel with a standard Gaussian kernel that uses a fixed bandwidth. Why would the forest’s neighborhood around x stretch along predictors that are irrelevant to the response and shrink along predictors that matter? How would changing mtry and node size reshape that neighborhood? Can you think of a situation where this adaptiveness is a weakness rather than a strength, for example with correlated predictors, points near the edge of the data, or regions where the training data are sparse?
