## Summary

[scDetect](https://github.com/IVDgenomicslab/scDetect/) is a new cell type ensemble learning classification method for single-cell RNA sequencing across different data platforms, using a combination of gene expression rank-based method and majority vote ensemble machine-learning probability-based prediction method.

To further accurate predict the tumor cells in the single cell RNA-seq data, we developed scDetect-Cancer, a classification framework which incorporated the cell copy number information and epithelial origin information in the classification.

## Application of scDetect

First, we load the [scDetect](https://github.com/IVDgenomicslab/scDetect/) package, and [Seurat](https://satijalab.org/seurat/install.html)

```markdown

library("scDetect")
library("Seurat")

```

We will work with single cell data from two human pancreas dataset. "Muraro" dataset were generated from CEL-Seq2 platform, "Xin" dataset were generated from SMARTer platform.

The count matrix and cell type lable of the test data could be obtained here.

Read the gene expression data and cell type lable.

```markdown

# Xin human pancreas dataset #
xin<-readRDS("~/xin_test.rds")
xin_lable<-readRDS("~/xin_test_lable.rds")

# Muraro human pancreas dataset #
muraro<-readRDS("~/muraro_test.rds")
muraro_lable<-readRDS("~/muraro_test_lable.rds")

```

## Training step

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/IVDgenomicslab/scDetect.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Feature genes identification

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
