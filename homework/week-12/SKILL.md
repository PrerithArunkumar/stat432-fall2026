---
name: cluster-select-report
description: Compare at least three clustering algorithms on a numeric data set, select one with label-free criteria, and write a one-page report that presents only the selected method. Use only when the user explicitly invokes this skill and supplies the data details.
disable-model-invocation: true
---

# Compare clustering methods and write a one-page report

The user supplies: the data file, the feature columns, any ID columns, any known
label columns, the language (R or Python), and the output format. Ask if any is missing.

## Rules

- Use only the feature columns for preprocessing, tuning, the number of clusters,
  and method selection. Known labels may be used only after the final method is
  chosen, to interpret its clusters.
- Fix and report every random seed. Every number in the report must come from
  code output; re-check each quoted number against the output after rendering.
- **The analysis report must fit on one printed page, including all text,
  figures, and tables.**
- **Present clustering results (assignments, cluster sizes, plots, profiles,
  label cross-tables) for only the one selected method.** The other methods may be
  named, with their selection-criterion values in one sentence or one short row
  each, to justify the choice. Do not show their partitions or plots.

## Steps

1. **Inspect.** Report n, p, missing values, and feature scales. Note whether
   p is much larger than n.
2. **Preprocess.** Handle missing values. If p is large (say p > 10n), keep the
   most variable features (for example the top 500) and say how many. Standardize
   features unless they share a meaningful common scale. Compute PCA and keep the
   smallest number of components explaining at least 70% of the variance
   (at most 10). Use these scores as the common space for every method, so the
   comparison does not depend on different inputs. PCA alone is not a clustering
   method.
3. **Candidates.** Fit at least three different algorithms, for each
   K = 2, ..., Kmax with Kmax = min(6, floor(n / 5)):
   - K-means with at least 50 random starts;
   - agglomerative hierarchical clustering with Ward linkage on Euclidean
     distance (average linkage may be added as a variant);
   - spectral clustering: Gaussian similarity on a symmetric k-nearest-neighbor
     graph (k about sqrt(n), bandwidth the median neighbor distance), normalized
     Laplacian, K-means on the row-normalized leading K eigenvectors;
   - optionally PAM or a Gaussian mixture.
4. **Criteria.** For every method and K compute, in the common PCA space:
   - mean silhouette width (Euclidean distance);
   - stability: over B >= 50 random subsamples of 80% of the observations, refit
     and compute the adjusted Rand index (ARI) with the full-data labels on the
     shared observations; report the mean;
   - the smallest cluster size.
5. **Select.** A solution is admissible if mean stability ARI >= 0.75 and the
   smallest cluster has at least max(3, 0.05 n) observations. For each method,
   take the admissible K with the largest silhouette. Choose the method with the
   largest silhouette; if two are within 0.02, prefer the higher stability, then
   the smaller K, then the simpler method (K-means, hierarchical, spectral). If no
   solution is admissible, say so and report the most stable one.
6. **Interpret the selected method.** Refit on all observations. Give cluster
   sizes and the features (or PCs) that differ most between clusters, using
   standardized mean differences. If labels were supplied, cross-tabulate
   clusters against them and give the ARI. State what the groups appear to
   represent and how confident the evidence allows one to be.
7. **Limitations.** Mention small n, the feature filter and PCA cutoff, the
   dependence on distance and scaling, stability, and that a 2-D display is not
   proof of separate populations.

## One-page report layout

- Title and one or two lines on the data and preprocessing.
- Methods and selection: name the algorithms, state the criteria and the rule, and
  give the selected method's values with one short line of comparison values.
- One figure for the selected method only, at most two panels and about 2.5 to 3
  inches tall: a scatter of the first two PCs colored by cluster (use point shape
  for known labels if supplied), plus a silhouette plot or a profile of the top
  distinguishing features.
- One small table: cluster sizes, or clusters by known label.
- Three to five sentences of interpretation, then one or two of limitations.
- Hide code on the report page; place code and any requested appendix (such as
  this skill) after a page break.
- Render, count the pages of the report section, and if it exceeds one page,
  shorten the text first, then reduce the figure. Re-render until it fits.
