---
layout: post
title: "由 reads count 数计算 TPM 和FPKM"
categories: 生物信息学
tags: RNAseq
description: RNAseq
author: biolxy
mathjax: false
---

#### 由 reads count 数计算 TPM 和FPKM
- 下载gff文件
- http://venanciogroup.uenf.br/cgi-bin/gmax_atlas/download_by_pub.cgi 去这里下载reads count 数据


```R
library("GenomicFeatures")
library("dplyr")
path1 = setwd("C:/Users/95656/Desktop/hyp/TZX/")
setwd(path1)
# 计算基因length

gffpath = "Gmax_275_Wm82.a2.v1.gene.gff3"
print(gffpath)
txdb <- makeTxDbFromGFF(gffpath,format="gff")
exons_gene <- exonsBy(txdb, by = "gene")
exons_gene_lens <- lapply(exons_gene,function(x){sum(width(reduce(x)))})
length1 = t(as.data.frame(exons_gene_lens))
write.table(length1,"C:/Users/95656/Desktop/hyp/TZX/length.txt", sep = "\t")

# 合并 reads count 数据和length
length1 = read.table("C:/Users/95656/Desktop/hyp/TZX/length.txt", header = T,sep="\t")
countsfile = "C:/Users/95656/Desktop/hyp/TZX/PRJNA238493_BP_raw_count.expression.tsv"
count1 = read.table(countsfile, header = T, sep = "\t")
a = inner_join(count1, length1, by="Locus_Name")
a[is.na(a)] = 0 # 设置 NA值为0
a = a[, -28]  # 删除第28列  Locus_Name.1
write.table(a,"C:/Users/95656/Desktop/hyp/TZX/countsdata.txt", sep = "\t", row.names = FALSE)

# 按照对应关系给countdata columns 改名，改好的文件保存为 re_countsdata.txt
# {'SAMN02644800': 'root', 'SAMN02644811': 'leaf3', 'SAMN02644826': 'seed4', 'SAMN02644825': 'seed3', 'SAMN02644822': 'podseed3', 'SAMN02644821': 'podseed2', 'SAMN02644820': 'podseed1', 'SAMN02644803': 'stem1', 'SAMN02644806': 'leafbud1', 'SAMN02644804': 'stem2', 'SAMN02644813': 'flower2', 'SAMN02644814': 'flower3', 'SAMN02644816': 'flower5', 'SAMN02644815': 'flower4', 'SAMN02644817': 'pod1', 'SAMN02644823': 'seed1', 'SAMN02644818': 'pod2', 'SAMN02644819': 'pod3', 'SAMN02644824': 'seed2', 'SAMN02644809': 'leaf1', 'SAMN02644807': 'leafbud2', 'SAMN02644802': 'cotyledon2', 'SAMN02644810': 'leaf2', 'SAMN02644805': 'shoot meristem', 'SAMN02644812': 'flower1', 'SAMN02644827': 'seed5'}

# 计算 TPM 和 FPKM

setwd("C:/Users/95656/Desktop/hyp/TZX/")
countfile = "C:/Users/95656/Desktop/hyp/TZX/re_countsdata.txt"
mycounts<-read.table(countfile, header = T,sep="\t")
head(mycounts)
rownames(mycounts)<-mycounts[,1]
mycounts<-mycounts[,-1]
head(mycounts)
# TPM
kb <- mycounts$length / 1000
countdata <- mycounts[,1:26]
rpk <- countdata / kb
tpm <- t(t(rpk)/colSums(rpk) * 1000000)
head(tpm)
write.table(tpm,file="my_TPM.txt",sep="\t",quote=F)
# FPKM
fpkm <- t(t(rpk)/colSums(countdata) * 10^6)
head(fpkm)
write.table(fpkm,file="my_FPKM.txt",sep="\t",quote=F)


## FPKM转化为TPM
# fpkm_to_tpm = t(t(fpkm)/colSums(fpkm))*10^6
# head(fpkm_to_tpm)


```

##### 计算 共表达相关性
```
path1 = "C:\\Users\\95656\\Desktop\\hyp\\比较"
setwd(path1)
rowdata <- read.table(choose.files(),header=T,sep="\t")
# exp1 = rowdata[,5:]
# "cotyledon1", "leafbud3",
exp1 = rowdata[c("cotyledon2", "flower1", "flower2", "flower3", "flower4", "flower5", "leaf1", "leaf2", "leaf3", "leafbud1", "leafbud2", "pod1", "pod2", "pod3", "podseed1", "podseed2", "podseed3", "root", "seed1", "seed2", "seed3", "seed4", "seed5", "shootmeristem", "stem1", "stem2")]
rownames(exp1)<-rowdata$Name
exp1[is.na(exp1)] = 0 # 设置 NA值为0
exp1 = as.matrix(exp1)
geneid <- rowdata[,'Name']
TCP = geneid[1:5] 
Gma = geneid[1:5]
outfile = choose.files()
for(i in 1:length(TCP))
{
    for(j in 1:length(Gma))
    {
        x <- as.numeric(exp1[i,])
        y <- as.numeric(exp1[j,])
        if(sum(exp1[i,])*sum(exp1[j,]) != 0)#取出表达值都为0的行，如果要严格一点，想要把表达值出现0的行都去掉的话可以把求和函数sum()换成求积函数prod(x)
        {
            gene_cor <- cor.test(x,y,alternative = "two.sided",method = "pearson")
            gene_p <- gene_cor$p.value
            gene_e <- gene_cor$estimate
            if(abs(gene_e) >= 0 && gene_p <= 1)
            {
                res<-paste(TCP[i],Gma[j],gene_e,gene_p,sep="\t")
                write.table(res, outfile,sep="\t",col.names=F,row.names=F,quote=F,append=T)
            }
        }else{
            print("something is error")
        }
    }
}


```

##### 用 python df 计算相关性

```python
import pandas as pd
from scipy import stats
import numpy as np


def get_dfcorr(df1, df2):
    """
    按行计算df1(gene1) 和 df2(gene2) 中数据的相关性, 其中 df1、2的列数可以不相等，method="pearson"
    100W 1m32s
    df 中 列为组织名称，行为基因名称
    """
    df1 = df1.loc[~(df1 == 0).all(axis=1), :]
    df2 = df2.loc[~(df2 == 0).all(axis=1), :]
    df1index = df1.index.tolist()
    df2index = df2.index.tolist()
    list1, list2 = df1.values.tolist(), df2.values.tolist()
    list_r = []
    for n, x in enumerate(list1):
        for m, y in enumerate(list2):
            a = np.array(x).astype(float)
            b = np.array(y).astype(float)
            r, p = stats.pearsonr(a, b)
            listtmp = [df1index[n], df2index[m], r, p]
            list_r.append(listtmp)
    df = pd.DataFrame(list_r, columns=["gene1", "gene2", "correlation", "pvalue"])
    return df
```