---
title: NVIDIA GPU and Cuda Introduction
tags:
  - Hardware
  - GPU
  - Cuda
categories:
  - Computer
  - Hardware
abbrlink: 6713c8a4
date: 2024-04-28 18:06:45
---

UPDATING

<!-- more -->

## Hardware Implementation

[Nvidia Document](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#hardware-implementation)

The NVIDIA GPU = a scalable array of multithreaded **Streaming Multiprocessors (SMs)**.

> Host CPU invokes a **kernel grid**, the blocks of the grid are enumerated and distributed to multiprocessors with available execution capacity. 

**thread block**: a group of threads, concurrently executs on one multiprocessor.

### SIMT: Single-Instruction Multiple-Thread

> The multiprocessor creates, manages, schedules, and executes in warps. 
> Thread blocks are partitioned into warps, each warp consecutively increases thread ID from 0.

**warp** = a group of 32 parallel threads.

**warp scheduler** = multiprocessor's scheduler of warps.

Critical property of warp:
1. A warp executes one common instruction at a time. 
2. If branch divergence occurs, warp executes one path first and disables threads are not on that path. 
3. Different warps execute independently.

> Comparision between SIMD and SIMT:
> SIMD vector expose the SIMD width to vector.
> SIMT specifies the execution and branching behavior of a single thread.
> **SIMT enables both thread-level parallel code for independent threads and data-parallel code for coordinated threads.**

