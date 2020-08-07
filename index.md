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

#### Prediction

To make scDetect easy to use, all steps were integrated into one function -- scDetect(). 

Here, we used Muraro pancreas dataset as the training dataset to predcit the cell types in Xin pancreas dataset. 


```markdown
# Using Muraro dataset as the training dataset #
vali_set_matrix<-xin
train_set_matrix<-muraro
train_set_lable<-muraro_lable

# Prediction #
prediction_results<-scDetect(vali_set_matrix,train_set_matrix,train_set_lable,p_value=0.5)
```

We can obtain a table showing the prediction results and detailed inforamtion.

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
# Melanoma reference dataset #
train_set_matrix<-readRDS("~/melanoma_reference.rds")
train_set_lable<-readRDS("~/melanoma_reference_lable.rds")

# Melanoma test dataset #
vali_set_matrix<-readRDS("~/melanoma_test.rds")
```

#### Prediction

To make scDetect-Cancer easy to use, all steps were integrated into one function -- scDetect-Cancer(). 

Here, we used Melanoma reference dataset (without tumor cells) as the training dataset to predcit the cell types in a melanoma test dataset. 

The gene position file used for single cell copy number variation analysis and gene list file used for epithelial score analysis could be obtained [here].



```markdown
# Prediction #
scDetect_Cancer_results<-scDetect_Cancer(vali_set_matrix,train_set_matrix,train_set_lable,gene_position_file,gene_list,output_dir)
```

We can obtain a list included the prediction results and detailed inforamtion.

The prediction results:

```markdown
scDetect_Cancer_results$lable[1:10]
##  "Tumor"   "Unknown" "Bcell"   "Unknown" "Unknown" "Unknown" "Tumor"   "Unknown" "Tumor"   "Bcell" 
```

The detailed inforamtion:


```markdown
scDetect_Cancer_results$detail_info[1:10,]
##                                        CNV_Class CNV_entropy_score anno_file Epithelial_score Epithelial_pvalue Epithelial_class   raw_lable final_lable
##cy81.Bulk.CD45.neg.B04.S112.comb           Tumor          7.935449     Other        0.2969218      6.743253e-99            Tumor  Fibroblast       Tumor
##cy94_cd45pos_4_C09_S33_comb                Other          7.940964     Other        0.1443036      1.000000e+00            Other     Unknown     Unknown
##cy72.CD45.pos.D04.S904.comb                Other          7.940905     Other        0.1684391      1.000000e+00            Other       Bcell       Bcell
##CY88CD45POS_2_D09_S429_comb                Other          7.942583     Other        0.2982854     7.294346e-100            Tumor       Tcell     Unknown
##CY75_1_CD45_CD8_1__S29_comb                Other          7.941474     Other        0.1341299      1.000000e+00            Other     Unknown     Unknown
##Cy80_II_CD45_F08_S932_comb                 Other          7.939729     Other        0.2424808      1.027392e-49            Tumor Endothelial     Unknown
##cy78.CD45.neg.2.C06.S606.comb              Tumor          7.936766     Other        0.3007999     1.283167e-101            Tumor  Fibroblast       Tumor
##CY94_CD45NEG_CD90POS_2_C02_S26_comb        Other          7.942693     Tcell        0.2200829      5.575592e-23            Tumor       Tcell     Unknown
##cy78.CD45.neg.2.A05.S581.comb              Tumor          7.933140     Other        0.3436351     3.547529e-127            Tumor  Fibroblast       Tumor
##cy79.p3.CD45.pos.PD1.neg.G01.S169.comb     Other          7.941923     Other        0.1801278      9.999995e-01            Other       Bcell       Bcell
```




