---
title: "按照图片高度拆分为多个图 DeepSeek（元宝） 按需求写代码，零修改"
categories:
  - Python
tags:
  - AI
authors: [转载]
date: 2025-05-30
---

# 按照图片高度拆分为多个图 DeepSeek（元宝） 按需求写代码，零修改

- 需求：写个python 脚本，读取图片，按照图片高度拆分为多个图，比如 输入5 时，按高度截取为5个子图

```python
import argparse
import os
from PIL import Image

def validate_arguments(input_path, num_split):
    if not os.path.isfile(input_path):
        raise FileNotFoundError(f"输入文件 {input_path} 不存在")
    if num_split <= 0:
        raise ValueError("拆分数量必须大于0")

def split_image(input_path, output_dir, num_split):
    # 验证参数有效性
    validate_arguments(input_path, num_split)
    
    # 打开原始图片
    img = Image.open(input_path)
    width, height = img.size
    
    # 检查拆分数量是否超过图片高度
    if num_split > height:
        raise ValueError(f"拆分数量({num_split})超过图片高度({height})")
    
    # 计算每个子图的高度分配
    split_height = height // num_split
    remainder = height % num_split
    
    # 生成高度列表（前remainder个子图高度+1）
    heights = [split_height + 1 if i < remainder else split_height 
               for i in range(num_split)]
    
    # 准备输出目录和文件名
    os.makedirs(output_dir, exist_ok=True)
    filename = os.path.basename(input_path)
    name, ext = os.path.splitext(filename)
    
    # 拆分并保存图片
    current_height = 0
    for i, h in enumerate(heights):
        # 计算裁剪区域
        box = (0, current_height, width, current_height + h)
        cropped_img = img.crop(box)
        
        # 生成输出路径
        output_path = os.path.join(output_dir, f"{name}_{i}{ext}")
        cropped_img.save(output_path)
        
        current_height += h

if __name__ == "__main__":
    # 配置命令行参数解析
    parser = argparse.ArgumentParser(description="将图片按高度拆分为多个子图")
    parser.add_argument("-i", "--input",  required=True, help="输入图片路径")
    parser.add_argument("-o", "--output", required=True, help="输出目录")
    parser.add_argument("-n", "--num_split", type=int, required=True, help="拆分数量")
    
    # 解析参数并执行拆分
    args = parser.parse_args()
    split_image(args.input, args.output, args.num_split)

```

## 运行

    python aa.py -i 1.jpg -o o -n 5
