---
title: "NEON SIMD"
subtitle: "Improved DPD (Digital Predistortion) algorithm performance by utilizing ARM's NEON registers"
excerpt: "Improved Digital Predistortion algorithm performance by utilizing NEON SIMD of ARM Cortex-A"
date: 2024-04-29
author: "Myung Guk Lee"
draft: false
images:
  - /career/assets/tachyons-thumbnail.png
  - /career/assets/tachyons-logo-script-feature.png
series:
  - Getting Started
tags:
  - hugo-site
categories:
  - Theme Features
# layout options: single or single-sidebar
layout: single
---


## In order to reduce the execution time of the DPD (Digital Predistortion) algorithm, the code was optimized to use NEON registers, which enable execution of multiple data at once.

### Background knowledges
1. DPD (Digital Predistortion)
Digital Pre-Distortion(DPD) is a technique used to compensate for nonlinearities introduced by power amplifiers(PAs). DPD works by applying a predistortion to the input signal before it passes through the PA,  effectively counteracting the nonlinear introduced by the amplifier. By predistortion the signal in a controlled manner, the output from the PA becomes more linear, resulting in improved signal quality.
![screenshot](/img/dpd.png)

2. ARM NEON
NEON is a SIMD(Single Instruction Multiple Data) architecture extension for the ARM processor. SIMD allows the same operation to be performed on multiple data in paralle, which can significantly improve performance for certain types of workloads.

The SoC is ARM Cortex-A series cores feature NEON-Coprocessor. This NEON coprocessor has 32 128-bit SIMD registers. Using these SIMD registers, parallel processing is possible as shown below.
![screenshot](/img/neonsimd.png)

### Implementation
1. Data Alignment
   To load multiple data into NEON registers, a NEON register size is is 128-bit. these data must be placed in contigous memory space. Additionally, DATA used in DPD is a complex type that combines real and image values. Since ARM armv-8 cannot perform operations on this complex type, dividing it into real and image parts must also be done.
![screenshot](/img/dividrealandimage.png)

2. Loop Unrolling & Unwinding
   Unrolling the loop and then grouping these unrolled iterations into a larger loop. This technique has several advantages:
   - Avoids branching,
   - Keep all temporary results into registers.
   - Data dependencies from previous versions help reduce pipeline latency.
![screenshot](/img/unrollingandunwinding.png)

3. Optimizing the matrix operations using 4x4 submatrices
   By unrolling and unwinding with a 4x4 submatrix, performance has been improved in matrix multiplication operations.
![screenshot](/img/4_4submatrix.png)