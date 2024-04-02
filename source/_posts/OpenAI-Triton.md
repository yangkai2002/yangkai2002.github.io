---
title: 'OpenAI: Triton'
tags:
  - MLSYS
  - Compiler
  - Operation
  - Triton
  - OpenAI
categories:
  - MLSYS
  - Compiler
abbrlink: 5f7cf5f5
date: 2024-04-01 20:30:26
---

学会使用和修改 OpenAI 的 Triton 深度学习编译器框架。

<!-- more -->

# Introduction

[Triton Github](https://github.com/openai/triton.git)

Triton 是一个用于深度学习的编译器框架，旨在提高深度学习模型的性能和效率。Triton 提供了一种新的方法，将深度学习模型编译为高效的机器码，以便在现代硬件上运行。

# Use

## Environment

使用 `conda` 管理环境，创建一个新环境：

```bash
conda create -n triton python=3.10
conda activate triton
pip install torch torchvision torchaudio
pip install triton
```

## Usage

使用方法参考 [Triton 官方文档](https://triton-lang.org/main/getting-started/installation.html)

# Development

## Environment

> 通过源码安装 Triton 的时候最好保证有科学上网

先获得 `Triton` 的源码：

```bash
git clone https://github.com/openai/triton.git
cd triton
```

使用 `conda` 管理环境，创建一个新环境：

```bash
conda create -n triton-dev python=3.10
conda activate triton-dev
```

安装基础环境和 clone 下来的 Triton 源码：

```bash
# ./triton
pip install torch torchvision torchaudio
pip install ninja cmake wheel
pip install -e python
```

如果发现出现缺少某些GCC可链接库，可以尝试如下方法：

```bash
conda install gcc -c conda-forge
```

## Build

如果能顺利安装好环境，接下来就可以开始对 Triton 进行修改了，修改完成之后重新执行下面的命令就可以将魔改过的 Triton 安装进 Conda 环境了：

```bash
# ./triton
pip install -e python
```

## Coding

Triton 源码的主要代码结构如下：

```bash
- python
    - src: 相关模块的 C++ 实现
    - triton
        - _C: 一些库和接口
        - backend: 不同硬件的后端
            - amd
            - nvidia
        - compiler: 编译器
        - language: triton.language
        - ...
```

可能出现的修改主要在`src`目录或者`triton`下的`backend`、`compiler`、`language`等目录下，因此其他的文件可以直接忽略。