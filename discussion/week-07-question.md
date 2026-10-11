---
id: w07-prerith2-standardize-before-pca
title: "Should the predictors be standardized before PCA when it is used as an embedding for LDA or QDA?"
author: prerith2
---

I am confused about whether to standardize the predictors before running PCA as a first step for LDA or QDA.
PCA finds the directions with the largest variance, so without standardization the first principal components
are dominated by whichever variables have the largest units. In the `SAheart` data, for example, `sbp` is
measured in mmHg and varies far more than `famhist`, which is coded only as 1 or 2.

1. Without standardization, would the first few PCs mostly just be the large-unit variables, so that a
   variable like `famhist` is thrown away before LDA or QDA ever sees it?
2. Does this choice change the LDA/QDA result much, and does it matter only when a smaller number $d$ of
   PCs is kept?
3. Is there a principled way to decide whether to standardize, or should we just compare both using
   cross-validation?
