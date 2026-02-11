Overview
This script compares two phylogenetic trees built from the same set of taxa:
LEFT tree: Structural guide tree (FoldMason; .nw)
RIGHT tree: Amino acid maximum-likelihood tree (IQ-TREE; .contree)
The goal is to evaluate whether structural similarity recapitulates amino acid sequence phylogeny and to visualize their congruence using a tanglegram.
What the Script Does
1. Reads Input Trees
Imports both trees using ape::read.tree().
Reports original tip counts.
2. Synchronizes Taxa
Retains only shared tip labels between trees.
Ensures identical taxon sets before comparison.
3. Resolves Topological Requirements
Checks whether each tree is fully bifurcating using is.binary().
Resolves polytomies with multi2di() only if necessary.
Applies ladderize() for consistent plotting order.
4. Converts Trees for Tanglegram Visualization
Because maximum-likelihood trees are not ultrametric, the script:
Computes cophenetic (patristic) distance matrices.
Performs hierarchical clustering (hclust()).
Converts results to dendrogram objects for use with dendextend.
This avoids ultrametric assumptions while preserving relative topology.
5. Defines Structural Clusters
Clusters are derived only from the LEFT (structural) dendrogram using a relative height cutoff:
h = 0.25 × maximum dendrogram height
These clusters define:
Tip label colors
Connecting line colors in the tanglegram
Cluster definitions are therefore structural in origin.
6. Generates a Tanglegram
The script:
Untangles trees to reduce line crossings.
Colors tips and connecting lines by LEFT cluster membership.
Forces branches to neutral gray (no edge highlighting).
Exports a high-resolution PNG figure.
Output:
~/Documents/AA_vs_Struct_tanglegram.png
Tree Comparison Metrics
Two quantitative congruence metrics are computed:
1. Normalized Robinson–Foulds (RF) Distance
RF.dist(unroot(tL), unroot(tR), normalize = TRUE)
Compares unrooted topology only
0 = identical trees
1 = completely different topology
2. Entanglement (dendextend)
Measures visual crossing complexity in the tanglegram.
0 = perfect alignment
1 = maximal crossing
Metrics are written to:
~/Documents/AA_vs_Struct_tree_metrics.csv
Biological Interpretation
This workflow tests:
Do structural similarity relationships mirror amino acid phylogeny?
Low RF + low entanglement → strong congruence
High RF → structural convergence or evolutionary decoupling
Required R Packages
ape
phangorn
dendextend
viridisLite
