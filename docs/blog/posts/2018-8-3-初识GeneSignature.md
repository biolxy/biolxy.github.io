---
layout: post
title: "初识Gene signature"
categories: 
  - 肿瘤基因组
tags: signature
keywords: 
  - Gene
  - signature
description: Gene signature
date: 2018-8-3
authors: [biolxy]
---


{:toc}






# 初识Gene signature


## 什么是Gene signature
[https://cancer.sanger.ac.uk/cosmic/signatures](https://cancer.sanger.ac.uk/cosmic/signatures)


## Gene signature 的原理
人类的参考基因组序列是固定的，可以由此计算出任意三个连续的碱基序列的先的频率

- 这样的三个连续的碱基ATG，TCG等共有96种
    + 密码子有64个，但是碱基组合有96（4 * 4 * 4 = 96）种
- 参考基因组的这96中序列的出现的频率也是固定的，以柱状图显示出来，即使原始signature
    + 图为cosmic数据计算出的一种signature1

![signature](https://cancer.sanger.ac.uk/signatures/Signature-1.png)

- 期吸烟的患者，体内基因发生变异，导致其测序统计后的signature与原始signature出现差异
- 统计大量上述数据，得出的signature即可视为吸烟患者的signaure，cosmic数据给出了30种signature1-30
- 测序获得一个新的signature-new,即可与signature1-30进行比较,从而解读该signature-new

## Gene signature的用途
预后诊断  


## 如何提取Gene signature

r语言，有两个两个包可以实现:

- [maftools](https://github.com/PoisonAlien/maftools/)  
    + 这个包功能比较全面，不局限在signature上
- [deconstrucSigs](https://github.com/raerose01/deconstructSigs)
    + 仅仅实现signature作图，但是可以把signature的权重提取出来，示例如下


### deconstrucSigs的安装
```r 
source("http://bioconductor.org/biocLite.R")
biocLite("")
```

### 输入数据格式
由病人的vcf文件中提取，整理为deconstrucSigs的格式  

```
Sample  chr      pos ref alt
1 chr1   905907   A   T
1 chr1  1192480   C   A
1 chr1  1854885   G   C
1 chr1  9713992   G   A
1 chr1 12908093   C   A
1 chr1 17257855   C   T
```
+ 注意decon.maf 中chr那一列为 chr1 ，不是 Chr1
+ sep = "\t"
+ 多个病人的数据`cat` 在一起，用sampleID做区分



#### 批量保存图片
其中 'XH01', 'XH11', 'XH12', 'XH19', 'XH28', 'XH31', 'XH33', 'XH34', 'XH36', 'XH38', 'XH39', 'XH40', 'XH43', 'XH44', 'XH45', 'XH47', 'XH49', 'XH51', 'XH52', 'XH54', 'XH56', 'XH58' 为我的数据中SampleID  
```r
suppressPackageStartupMessages(library("deconstructSigs"))
sigs.input <- mut.to.sigs.input(mut.ref="decon.maf",sample.id = "Sample", chr = "chr", pos = "pos", ref = "ref", alt = "alt")
l = list('XH01', 'XH11', 'XH12', 'XH19', 'XH28', 'XH31', 'XH33', 'XH34', 'XH36', 'XH38', 'XH39', 'XH40', 'XH43', 'XH44', 'XH45', 'XH47', 'XH49', 'XH51', 'XH52', 'XH54', 'XH56', 'XH58')
pdf("plot.cosmic.pdf")
for(i in l){
test = whichSignatures(tumor.ref = sigs.input,signatures.ref = signatures.cosmic,sample.id=i,signature.cutoff=0,contexts.needed = TRUE,tri.counts.method = 'default')
plotSignatures(test)
makePie(test)
}
dev.off()
```
+ 该方法有个缺陷，图片输出的title字体太大，当signature较多时显示不完全


#### 批量输出各signature 的weights
```r 
for(i in l){
    test <- whichSignatures(tumor.ref = sigs.input,signatures.ref = signatures.cosmic,sample.id=i,signature.cutoff=0,contexts.needed = TRUE,tri.counts.method = 'default')
    res <- test$weights
    write.table(res,"cosmic.weight.txt",sep="\t",col.names=F,row.names=T,quote=F,append=T)
}
```

- 可以将 `signatures.ref = signatures.cosmic` 改为 `signatures.ref = signatures.signatures.nature2013` ,这是两个不同来源的数据集算出的signature，我们用计算出的signature 与该数据的signature进行比较，为我们的数据做注释
    + 得出的结果是：signature_sample1 = signature1 * weight1 + signature2 * weight2 + ··· + signature30 * weight30
    + 通过权重的比值对我们算出的signature进行注释



