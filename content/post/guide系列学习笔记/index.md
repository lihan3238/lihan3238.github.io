---
title: guide系列学习笔记
description: 使用新的模式进行 guide 系列学习笔记的记录
slug: guide_study_notes
date: 2025-09-14 09:14:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - guide
# math: 
# license: 
hidden: false
comments: true
#weight: 1  # light forward; heavy backward
draft: false

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了
---

# 前言———— guide 系列学习笔记

从这篇帖子开始，我计划设计并沿用新的模式来记录 [`guide`](https://lihan3238.github.io/tags/guide) 类的学习笔记。

## 为什么要设计新的模式？

过去的技术贴中普遍存在以下问题：
- 笔记内容零散，缺乏系统性和连贯性，仅为具体代码用法的备忘
- 缺乏统一的格式和结构，阅读体验不佳
- 想到什么学什么的习惯难以进行一种新的技术的系统性学习

## 新的模式

我希望统一出一个学习笔记系列的模板主要有以下功能：
1. 继承之前帖子的备忘学习和开发查阅；
2. 为了系统性学习，加强各个技术开发贴之间的联系，设计两种模板：
    a. 【方向A】的系统学习记录,例如 深度学习 学习笔记；
    b. 【具体技术 I】的开发实操学习记录。a类帖子是系统性学习一个方向的主体，a中会链接到若干个其方向依赖的技术的b类帖子。ab类帖子的关系为多对多，即b类帖子可被重复链接。

## 具体模板

```md
---
title: 【I方向】 系统学习笔记   # 修改为方向名称
description: 系统性学习记录【I方向】，包含概念辨析、路线、依赖、资料与学习过程
slug: notes_A_I
date: 2025-09-14 00:00:00+0800
image: imgs/avatar.png   # 可改
categories:
    - techStudy
tags:
    - 【I方向】
    - guide_A
hidden: false
comments: true
draft: false
# math: 
# license: 
#weight: 1  # light forward; heavy backward

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了

---

# 【I方向】 学习笔记

## 1. 基础概念

### 1.1 介绍

### 1.2 相关概念

---

## 2. 学习路线规划

| 阶段 | 学习目标 | 依赖 |
|------|------|-------------------|
| **基础阶段** | 熟练使用 Python，掌握数据结构与函数式编程 | [Python 基础笔记](/posts/python_notes/)，《Python编程：从入门到实践》 |
|              | 线性代数（矩阵运算、特征值）、微积分（偏导数、链式法则）、概率统计 | [NumPy 学习记录](/posts/numpy_notes/)，3Blue1Brown 视频系列 |
|              | Git 基础操作，Jupyter Notebook，虚拟环境管理（conda） | [Git 实操笔记](/posts/git_notes/)，官方文档 |
| **进阶阶段** | 熟悉 PyTorch 的张量操作、自动微分、nn.Module | [PyTorch 实操笔记](/posts/pytorch_notes/)，PyTorch 官方教程 |
|              | 理解感知机、MLP，掌握 CNN、RNN 的基本结构与实现 | CS231n 视频课程，论文：《LeNet》《AlexNet》 |
|              | 学会数据增强、正则化、学习率调度，能处理过拟合 | [数据增强笔记](/posts/data_aug_notes/) |
| **应用阶段** | 能独立完成一个分类/回归任务（图像/文本） | Kaggle 入门项目，fast.ai 实战课程 |
|              | 掌握模型保存与加载，使用 Docker 部署推理服务 | [Docker 实操笔记](/posts/docker_notes/)，《Hands-On Machine Learning》 |
|              | 阅读 Transformer 相关论文，复现小规模实验 | 《Attention is All You Need》，[Hugging Face 笔记](/posts/hf_notes/) |


---

## 3. 技术依赖
- [Python 基础笔记](/posts/python_notes/)  
- [NumPy 学习记录](/posts/numpy_notes/)  
- [PyTorch 实操笔记](/posts/pytorch_notes/)  

---

## 4. 相关资料链接
- [Deep Learning 官方书籍](https://www.deeplearningbook.org/)  
- [Stanford CS231n 课程](http://cs231n.stanford.edu/)  
- [优秀博客A](https://xxx)  

---

## 5. 学习过程记录
### 2025-09-14
- 学习完绪论，整理了基础概念  

### 2025-09-20
- 完成第一个 CNN 实验，遇到梯度消失问题，使用 ReLU 解决  

---

```

- [0_方向系统学习记录模板A](https://lihan3238.github.io/p/notes_A_I)

```md
---
title: 【x技术】 学习笔记   # 修改为方向名称
description: 学习记录【x技术】，记录开发学习过程
slug: notes_B_x
date: 2025-09-14 00:00:00+0800
image: imgs/avatar.png   # 可改
categories:
    - techStudy
tags:
    - 【x技术】
    - guide_B
hidden: false
comments: true
draft: false
# math: 
# license: 
#weight: 1  # light forward; heavy backward

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了

---

# PyTorch 实操笔记

## 1. 简单介绍
- PyTorch 是一个深度学习框架，主要用于研究和生产环境  
- 常见应用场景：图像识别、自然语言处理、生成模型  

---

## 2. 前置技术依赖
- [Python 基础笔记](/posts/python_notes/)  
- [NumPy 学习记录](/posts/numpy_notes/)  

---

## 3. 环境配置
```bash
conda create -n dl python=3.10
conda install pytorch torchvision torchaudio -c pytorch
```

---

## 4. 基础用法示例

```python
import torch
x = torch.rand(5, 3)
print(x)
```

---

## 5. 常见问题与解决办法

* **显存不足** → 使用 `torch.cuda.empty_cache()` 或减小 batch size
* **安装版本冲突** → 确认 CUDA 版本与 PyTorch 官方对应表

---

## 6. 实战经验

* 使用 `DataLoader` 提升数据处理效率
* 训练时使用 `tensorboard` 可视化 loss 和 accuracy

---

## 7. 参考资料

* [PyTorch 官方文档](https://pytorch.org/docs/stable/index.html)
* [fast.ai 课程](https://course.fast.ai/)

---



```

- [0_技术工具学习记录模板B](https://lihan3238.github.io/p/notes_B_x)