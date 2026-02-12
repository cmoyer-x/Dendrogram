# Structural vs. Sequence Phylogeny: Tanglegram Comparison

Compare a **structural guide tree** (FoldMason) against an **amino acid maximum-likelihood tree** (IQ-TREE) to assess whether protein structural similarity recapitulates sequence-based phylogeny.

The script produces a publication-ready tanglegram and computes quantitative congruence metrics between the two trees.

---

## Workflow

### 1. Input & Preprocessing

Both trees are read via `ape::read.tree()` and synchronized to a shared taxon set. Polytomies are resolved with `multi2di()` only when necessary, and trees are ladderized for consistent visual ordering.

### 2. Dendrogram Conversion

Because maximum-likelihood trees are not ultrametric, the script avoids ultrametric assumptions entirely. Instead, it computes cophenetic (patristic) distance matrices, performs hierarchical clustering (`hclust()`), and converts the results to `dendrogram` objects compatible with **dendextend**. This preserves relative topology without imposing a molecular clock.

### 3. Structural Clustering & Coloring

Clusters are derived exclusively from the **left (structural) dendrogram** using a relative height cutoff:

```
h = 0.25 × max(dendrogram height)
```

These structural clusters define tip label colors and connecting-line colors in the tanglegram. Branch edges are rendered in neutral gray to keep the focus on cross-tree correspondence.

### 4. Tanglegram Generation

The script untangles the two dendrograms to minimize line crossings, then exports a high-resolution PNG:

```
~/Documents/AA_vs_Struct_tanglegram.png
```

---

## Congruence Metrics

Two metrics are computed and written to `~/Documents/AA_vs_Struct_tree_metrics.csv`:

| Metric | Method | Interpretation |
|--------|--------|----------------|
| **Normalized Robinson–Foulds distance** | `RF.dist(unroot(tL), unroot(tR), normalize = TRUE)` | Topological similarity on unrooted trees. 0 = identical, 1 = completely different. |
| **Entanglement** | dendextend tanglegram score | Visual crossing complexity. 0 = perfect alignment, 1 = maximal crossing. |

---

## Biological Interpretation

This workflow tests a core question: **do structural similarity relationships mirror amino acid phylogeny?**

- **Low RF + low entanglement** → strong congruence between structure and sequence evolution
- **High RF** → possible structural convergence, evolutionary decoupling, or limitations of the alignment method

---

## Requirements

| Package | Purpose |
|---------|---------|
| `ape` | Tree I/O, manipulation, and ladderization |
| `phangorn` | Robinson–Foulds distance |
| `dendextend` | Tanglegram construction and entanglement scoring |
| `viridisLite` | Colorblind-friendly cluster palette |
