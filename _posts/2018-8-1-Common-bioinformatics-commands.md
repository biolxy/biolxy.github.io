---
layout: post
title: "生物信息学常用的Linux命令"
categories: 生物信息学
tags: 生物信息学 肿瘤基因组
keywords: Linux shell sed wak 
description: 这是我最初接触生物信息学时用到的命令，未仔细整理
---

* content
{:toc}

### 构建blast数据库
```shell
makeblastdb -in ../ATaa.fa -dbtype prot -title ATaa-parse_seqids -out ATdb   #可用的蛋白库
makeblastdb -in ../ATaa.fa -dbtype nucl -title ATaa-parse_seqids -out ATdb  #可用的核酸白库
blastp -db /home/hanyapeng/prot/DB/Mtrdb -query At.fa -outfmt 6 -evalue 1e-20 -out Mtr.blast -num_threads 8
blastn -query seq.fasta -out seq.blast -db dbname -outfmt 6 -evalue 1e-5 -num_descriptions 10 -num_threads 8
```   

### 碱基/蛋白序列比对
```
muscle -in AtLj.fa -out AtLj.phy  # -out，输出默认的fasta格式
muscle -in At.fasta -phyiout At.phy  # -phyiout，则输出sequential phylip格式
mafft aa.fa > aa.fas
```

### 画树
```shell
nohup raxmlHPC-PTHREADS -s ALO.phy -n ALO -m PROTCATJTT -f a -N 100 -p 12345 -x 1234567 -T 8 &  # RaxML树
nohup ../software/PhyML_3.0_linux64 -i AGO.phy -d aa -b 0  -f e -s BEST -c 4 -m VT -v e -a e -o lr --no_memory_check &  # Phyml 树
../software/FastTree TCP123.phy  > TCP.tree   #fasttree 画树
```

### 提取CDS/pep序列
```shell
nohup hmmsearch -E 1e-5 TCP.hmm  /home/hanyapeng/prot/Gma.prot > Gma.hmm &
less -S Gma.hmm |awk '{if ($1~/e-/) print $9}' > list     #if第一行匹配到e-，则调出第9行#  

nohup hmmsearch --domtblout hmmresult.txt -E 1e-5 TCP.hmm  /home/hanyapeng/prot/Gma.prot  &
less -S hmmresult.txt |awk '{ if ($1~/e-/) print$1"\t"$20"\t"$21}'|less -S > GmaTCPdomain.list    # 提取第1.20.21列为坐标

perl /home/lixiangyong/software/getseq.pl Osa.list /home/hanyapeng/prot/Osa.prot > Osa.fa
perl /home/lixiangyong/software/get_domain.pl  /home/hanyapeng/prot/Osa.prot  OsaTCPdomain.list > OsaTCPdomain.fa  #提取domain

perl /home/lixiangyong/software/getseq.pl Chr13 Gmax275.fa > Chr13.fa  
perl /home/hanyapeng/comm/getpromoter.pl /data/plant_genome/Glyma2.0/Gmax275.fa Q1.txt > 1  # 提取启动子序列 Q1为启动子坐标
cat AT.fa Lj.fa Osa.fa > ATLjOsa.fa
```


### 修改环境变量
```
vi .bash_profile     # 或则 .bash_login   .profile   .bashrc 
vi .bash_profile     # 添加环境变量 PATH=$PATH:$HOME/bin
source .bash_profile  #   立刻执行更改

nano .bashrc             #和vi类似的编辑器
source .bashrc
```



### prottest的使用
```
nohup java -jar prottest-3.4.jar -i AGO.phy -all-matrices -all-distributions  > a.txt  &  # 此.phy 文件为比对过的氨基酸序列文件

nohup java -jar prottest-3.4.jar -i namefail.phy -o namefail.txt -S 1 -AIC -I -G -IG -F -ncat 8 -all &

nohup java -jar prottest-3.4.jar -i AGOdomain.phy -o AGOdomain.txt -all-matrices -all-distributions &

nohup java -jar jModelTest.jar -d example-data/aP6.fas -f -i -g 4 -s 11 -AIC -a >a.txt  &  此.fas文件为核苷酸序列文件

PhyML-3.1_linux64 -i aP6.phy -d nt -n 1 -b 0 --run_id F81+I -m 000000 -f m -v e -c 1 --no_memory_check -o tlr -s BEST

PhyML-3.1_linux64 -i aP6.phy -q -d aa -m JTT -c 4 -a e
```



### shell 脚本知识
```shell
!= 不等于,如:if [ "$a" != "$b" ] 
```



### awk的使用
```shell
less Y2.blast |awk ' {if ($11 ~ /0\.0/) print $0}' > 1      # 每一行中，如果第11列=0.0，则输出全部  “~”是 =  ，$11 调取第11列 ， \ 反义符 
awk '{line[NR]=$0}END{for(i=NR;i>0;i--)print line[i]}' sed.txt  # 把sed.txt 中的内容按行倒着输出
less -S Gma3.fa |awk '{if ($1~/>/) print $head}' > a
awk '!seen[$0]++' <filename>    # awk 去除重复的行
less Step1.2.sh | grep "=" | cut -d "=" -f 1 | awk '{printf $quot" ";$0}'  # 将一列输出为一行
```


### sed的使用
```shell
sed -i 's/Gma/Glyma/g' Glyma.fa
sed -i '/^$/d' 2_domain.txt  # 删除文件空行
sed -i '/#/d' AtTCP.txt      # 删除有\#的行
sed -i s/^M// AAt.list.txt   # 此处^M必需手动输入！！
sed -i /^$/d file     #删除空行   d 指删除含有空行符的这一行
sed -i s/tr.*//g LjTCP.fa  #  去除无用的注释    :%s/|.*.\[//g  #去除从|开始，到[结束内的所有字符，包括[#

sed 's/^/aaa/g' filename.txt > outputfile.txt  # 在文件中的每一行之前加上一个字符串，比如：aaa    行尾将^是 $ 
sed -i/\>Chr17/d Chr17.fa   # 删除Chr17.fa 中的Chr字符
sed -i "s/zhangsan/lisi/g" `grep zhangsan -rl modules` #批量替换，讲文件夹modules中所有文件执行sed -i       "s/zhangsan/lisi/g" 该符号`不是点，是Esc下面的符号 
sed -i '1iNew Top Line' aa.txt    #  sed 在句首添加一行 New Top Line
```



### linux 命令
```shell
date   #  查看系统日期
cal 1 2016   # 查看2016年1月的日历
df     # 查看磁盘驱动器的可用空间  
free   # 显示可用内存
.     # 工作目录     
..   # 工作目录的父目录   如：  cd ..   返回上一级    cd  ./software/  
file Gma1.fa   # 显示  Gma1.fa: ASCII text  该文件为文本文档ASCII
alias foo='cd /usr; ls; cd -'   # 自己创建命令   unalias foo 删除
gedit Gma1.fa   # 图形编辑器编辑Gma1.fa 

rm -- -foo   # 要删除第一个字符为‘-’的文件 (例如‘-foo’)
rm ./-foo   # 要删除第一个字符为‘-’的文件 (例如‘-foo’)

chmod 755 lixiangyong/  # 更改目录权限为可读

wc -l filename # 就是查看文件里有多少行
wc -w filename # 看文件里有多少个word。
wc -L filename # 文件里最长的那一行是多少个字
wc -c  filename   # 统计字节数

cat Y.list | sort -u > YRedundancy.list    # 去除文件Y.list中的重复的行命名为YR.list
less -S AGO.fa |grep ">" |wc -l  # 检索>有多少个  同  grep -c ">" AGO.fa
for tar in *.tar.gz;  do tar xvf $tar; done    #批量解压文件#
split -l 10 splitfile.txt D  # 将文件按每10行分割，新文件命名为 Daa Dab#
rename linux unix linux*    # 文件批量  
find . -name '*.txt' | xargs perl -pi -e 's/aaa/bbb/g'  # 遍历整个文件夹，把所有txt文件中的aaa替换成bbb
mkdir $(date +%F)  #创建一个以日期为名的文件夹
touch {1..9}.txt   # 批量创建文件

ls -F | grep '/$'         #  查看当前目录下的文件夹 

rename "s/AA/aa/" *
rename .fas .fa *.fas    # CentOS 下可用
```

### 单行 perl
```
perl -e "print lc($a)"    # Perl 大小写转换
```


### R 命令行
```
Rscript /home/muscle/test.r  # linux 上运行r脚本
```


### kaks计算
```
perl pal2nal.perl aligned.protein.fas original.cds.fas -output fasta > aligned.cds.fas
perl ../software/pal2nal.pl 2.fas 2.cds -output paml > 2.nuc    #kaks计算
```


### gff和gtf之间的格式转化
```  
gffread my.gff3 -T -o my.gtf # gff2gtf
gffread merged.gtf -o- > merged.gff3  # gtf2gff
```


### vim 的使用

```
:12,23s/aa/bb/g     # 将从12 行到23 行中出现的所有包含aa 的字符串中的aa 替换为bb
:s/\<aa\>/bb/g      # 将光标所在行出现的所有aa 替换为bb, 仅替换aa 这个单词
:12,23s/^/#/        # 将从12 行到23 行的行首加入# 字符

:10,15s#^#\#\##g          #  vim的 V模式下，下注释x  10 -15  行，##注释

Esc shift+ : wq   enter   =  Esc shift+ : x  enter       #保存并退出
Esc shift+ : q！  enter                                  #不保存强制退出 
vi foo.txt ls-output.txt    # 同时vi编辑多个文件  ：n   ：N   ：buffer 2 来回切换     也可以先打开一个，在用：e ls-output.txt
```


### 数据存放
/data/public/hanyapeng/Gm/Gma.collinear.groups      # 大豆中所有旁系同源基因  
/data/public/wanglei/inparanoid4.0/Soybeanparalog2.txt     # 大豆中所有旁系同源基因