---
layout: post
title: "PyClone 安装，报错及解决"
categories: Linux
tags: 肿瘤基因组 癌症基因组
description: rename的介绍
author: biolxy
mathjax: true
---

* content
{:toc}

## PyClone 安装，报错及解决

### 安装

``` shell
conda install PyClone
```

命令如下：

```shell
PyClone run_analysis_pipeline --in_files SRR385938.tsv SRR385939.tsv SRR385940.tsv SRR385941.tsv --working_dir ./tt
```

### 报错：

```shell
Traceback (most recent call last):
  File "/home/lixy/miniconda2/envs/ngs2/bin/PyClone", line 11, in <module>
    load_entry_point('PyClone==0.13.1', 'console_scripts', 'PyClone')()
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/pyclone/cli.py", line 78, in main
    args.func(args)
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/pyclone/run.py", line 97, in run_analysis_pipeline
    args.thin
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/pyclone/run.py", line 409, in _cluster_plot
    samples=samples,
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/pyclone/post_process/plot/clusters.py", line 58, in density_plot
    fig = pp.figure(figsize=(width, height))
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/pyplot.py", line 534, in figure
    **kwargs)
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/backend_bases.py", line 170, in new_figure_manager
    return cls.new_figure_manager_given_figure(num, fig)
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/backend_bases.py", line 176, in new_figure_manager_given_figure
    canvas = cls.FigureCanvas(figure)
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/backends/backend_qt5agg.py", line 35, in __init__
    super(FigureCanvasQTAggBase, self).__init__(figure=figure)
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/backends/backend_qt5.py", line 235, in __init__
    _create_qApp()
  File "/home/lixy/miniconda2/envs/ngs2/lib/python2.7/site-packages/matplotlib/backends/backend_qt5.py", line 122, in _create_qApp
    raise RuntimeError('Invalid DISPLAY variable')
RuntimeError: Invalid DISPLAY variabl
```

### 解决：
aroth85 ：
> Please check user group (<https://groups.google.com/forum/#!forum/pyclone-user-group>) for threads on this.  



**具体：** 

1. 报错的原因是 matplotlib的配置文件没有设置好：
2. 找到配置文件：

```shell
>>> import matplotlib
>>> matplotlib.matplotlib_fname()
'/home/foo/.config/matplotlib/matplotlibrc'
```

1. 编辑你的 matplotlibrc (第44行) ：

```shell
vim /home/foo/.config/matplotlib/matplotlibrc :
# backend      : qt5agg
backend      : Agg
```

再次执行命令，成功！