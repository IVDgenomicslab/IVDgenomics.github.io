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

## Prediction

To make scDetect easy to use, all steps were integrated into one function -- scDetect(). 
Here, we used Muraro pancreas dataset as the training dataset to predcit the cell types in Xin pancreas dataset. By default, scDetect() uses a threshold of p value 0.1 to classify the cells into categories.


```markdown
# Using Muraro dataset as the training dataset #
vali_set_matrix<-xin
train_set_matrix<-muraro
train_set_lable<-muraro_lable

# Prediction #
prediction_results<-scDetect(vali_set_matrix,train_set_matrix,train_set_lable,p_value=0.1)

```

We can obtain a contingency table showing the prediction results versus the true cell type labels.

The prediction results of scDetect included four columns:
predict_lable: Predicted cell type of the highest predict_score cell type;
predict_score: Highest predict score of the corresponding cell type;
pvalue: p value of the predict score based on the permutation analysis;
final_predict_lable: Predicted cell type based on the predict score and pvalue.


```markdown
##           predict_lable     predict_score pvalue final_predict_lable
##Sample_1           beta  0.44047619047619  0.512             Unknown
##Sample_2           beta 0.452380952380952  0.439                beta
##Sample_3           beta 0.619047619047618   0.08                beta
##Sample_4           beta 0.571428571428572  0.141                beta
##Sample_6           beta 0.666666666666667  0.059                beta
##Sample_7           beta 0.452380952380952  0.439                beta
##Sample_9           beta               0.5  0.348                beta
##Sample_12          beta 0.416666666666667  0.548             Unknown
##Sample_13          beta               0.5  0.348                beta
##Sample_14          beta 0.738095238095238  0.012                beta
##Sample_15          beta 0.833333333333333  0.006                beta
##Sample_16          beta 0.619047619047618   0.08                beta
##Sample_17          beta 0.571428571428572  0.141                beta
##Sample_18          beta 0.369047619047618  0.708             Unknown
##Sample_19          beta 0.654761904761905  0.059                beta
##Sample_20          beta 0.523809523809523  0.263                beta
##Sample_21          beta 0.523809523809523  0.263                beta
##Sample_24          beta 0.619047619047618   0.08                beta
##Sample_25          beta 0.404761904761905  0.572             Unknown
##Sample_26          beta 0.571428571428572  0.141                beta
```


## Application of scDetect-Cancer

For the single cell RNA-seq data of the tumor samples. First, we load the [scDetect](https://github.com/IVDgenomicslab/scDetect/) package, and [Seurat](https://satijalab.org/seurat/install.html)

```markdown

library("scDetect")
library("Seurat")

```

We will work with single cell data from a test melanoma dataset. 

The count matrix and cell type lable of the test data could be obtained [here].

Read the gene expression data and cell type lable.

```markdown

# Xin human pancreas dataset #
xin<-readRDS("~/xin_test.rds")
xin_lable<-readRDS("~/xin_test_lable.rds")

# Muraro human pancreas dataset #
muraro<-readRDS("~/muraro_test.rds")
muraro_lable<-readRDS("~/muraro_test_lable.rds")

```

## Prediction

To make scDetect easy to use, all steps were integrated into one function -- scDetect(). 
Here, we used Muraro pancreas dataset as the training dataset to predcit the cell types in Xin pancreas dataset. By default, scDetect() uses a threshold of p value 0.1 to classify the cells into categories.


```markdown
# Using Muraro dataset as the training dataset #
vali_set_matrix<-xin
train_set_matrix<-muraro
train_set_lable<-muraro_lable

# Prediction #
prediction_results<-scDetect(vali_set_matrix,train_set_matrix,train_set_lable,p_value=0.1)

```

We can obtain a contingency table showing the prediction results versus the true cell type labels.

The prediction results of scDetect included four columns:
predict_lable: Predicted cell type of the highest predict_score cell type;
predict_score: Highest predict score of the corresponding cell type;
pvalue: p value of the predict score based on the permutation analysis;
final_predict_lable: Predicted cell type based on the predict score and pvalue.


```markdown
##           predict_lable     predict_score pvalue final_predict_lable
##Sample_1           beta  0.44047619047619  0.512             Unknown
##Sample_2           beta 0.452380952380952  0.439                beta
##Sample_3           beta 0.619047619047618   0.08                beta
##Sample_4           beta 0.571428571428572  0.141                beta
##Sample_6           beta 0.666666666666667  0.059                beta
##Sample_7           beta 0.452380952380952  0.439                beta
##Sample_9           beta               0.5  0.348                beta
##Sample_12          beta 0.416666666666667  0.548             Unknown
##Sample_13          beta               0.5  0.348                beta
##Sample_14          beta 0.738095238095238  0.012                beta
##Sample_15          beta 0.833333333333333  0.006                beta
##Sample_16          beta 0.619047619047618   0.08                beta
##Sample_17          beta 0.571428571428572  0.141                beta
##Sample_18          beta 0.369047619047618  0.708             Unknown
##Sample_19          beta 0.654761904761905  0.059                beta
##Sample_20          beta 0.523809523809523  0.263                beta
##Sample_21          beta 0.523809523809523  0.263                beta
##Sample_24          beta 0.619047619047618   0.08                beta
##Sample_25          beta 0.404761904761905  0.572             Unknown
##Sample_26          beta 0.571428571428572  0.141                beta
```
