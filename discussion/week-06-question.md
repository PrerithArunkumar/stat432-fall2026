---
id: w06-prerith2-why-smaller-tree-predicts-better
title: "How can a pruned tree with higher training error predict new observations better?"
author: prerith2
---

I am confused about why pruning helps. Pruning removes splits, so the smaller tree has fewer leaves and a higher
training error than the full tree. I don't see how making the fit to the training data worse can make predictions
on new observations better.

I noticed something that might be similar in Homework 06: KNN with $k = 1$ had a training AUC of $1.000$ but a test
AUC of only $0.876$, while $k = 20$ had a lower training AUC ($0.988$) and a higher test AUC ($0.959$).

1. Is a fully grown tree failing in the same way as 1NN, with leaves so small that each leaf prediction is based on
   only a few training points and mostly reflects their noise?
2. If so, which splits does pruning remove: those that reduce the training RSS or Gini index only a little, and
   only for a few observations?
3. How do we know when we have pruned too much, so that the tree starts missing real structure in the data?
