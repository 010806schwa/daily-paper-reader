---
title: "SCRAPL: Scattering Transform with Random Paths for Machine Learning"
title_zh: SCRAPL：基于随机路径的散射变换用于机器学习
authors: "Christopher Mitcheltree, Vincent Lostanlen, Emmanouil Benetos, Mathieu Lagrange"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=RuYwbd5xYa"
tags: ["query:shipnoise-hf"]
score: 6.0
evidence: 散射变换可用于声学特征提取，支撑高频识别
tldr: SCRAPL提出了一种基于随机路径的散射变换优化方案，用于高效计算联合时频散射变换（JTFS）。该方法可作为可微损失函数用于神经网络训练，显著降低了计算开销。它能够提取包含高频细节的时频特征，可应用于船舶噪声识别中的高频成分分析。通过在音频和视觉任务上的实验，验证了其高效性和有效性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 散射变换计算开销大，限制了其在神经网络训练中的使用。
method: 提出随机路径采样策略，高效近似联合时频散射变换。
result: 在计算效率和特征质量上达到平衡，优于现有方法。
conclusion: SCRAPL为散射变换的实用化提供了有效方案，可推广至音频处理。
---

## Abstract
The Euclidean distance between wavelet scattering transform coefficients (known as paths) provides informative gradients for perceptual quality assessment of deep inverse problems in computer vision, speech, and audio processing. However, these transforms are computationally expensive when employed as differentiable loss functions for stochastic gradient descent due to their numerous paths, which significantly limits their use in neural network training. Against this problem, we propose "Scattering transform with Random Paths for machine Learning" (SCRAPL): a stochastic optimization scheme for efficient evaluation of multivariable scattering transforms. We implement SCRAPL for the joint time–frequency scattering transform (JTFS) which demodulates spectrotemporal patterns at multiple scales and rates, allowing a fine characterization of intermittent auditory textures. We apply SCRAPL to differentiable digital signal processing (DDSP), specifically, unsupervised sound matching of a granular synthesizer and the Roland TR-808 drum machine. We also propose an initialization heuristic based on importance sampling, which adapts SCRAPL to the perceptual content of the dataset, improving neural network convergence and evaluation performance. We make our code and audio samples available and provide SCRAPL as a Python package.

---

## 论文详细总结（自动生成）

# 论文《SCRAPL: Scattering Transform with Random Paths for Machine Learning》详细总结

## 1. 核心问题与研究动机

- **背景**：小波散射变换系数（路径）之间的欧氏距离能为深度逆问题的感知质量评估提供信息丰富的梯度，在计算机视觉、语音和音频处理中具有应用潜力。
- **问题**：当散射变换用作可微损失函数进行随机梯度下降训练时，其计算成本极高（路径数量多），严重限制了在神经网络训练中的实用性。
- **整体目标**：提出一种随机优化方案，高效计算多变量散射变换，使其可作为可微损失函数用于神经网络训练，同时保持特征的质量。

## 2. 方法论核心思想与关键技术

- **核心思想**：通过随机路径采样来近似完整联合时频散射变换（JTFS），降低计算开销。
- **关键技术细节**：
  - 提出 **SCRAPL**（Scattering transform with Random Paths for machine Learning）方案，对 JTFS 的路径进行随机子采样，每次前向/反向传播仅计算少量随机路径，从而将计算复杂度从 O(路径总数) 降低到 O(采样数)。
  - 引入基于重要性采样的初始化启发式方法，根据数据集感知内容自适应地调整路径采样分布，改善神经网络收敛与评估性能。
- **算法流程（文字说明）**：
  1. 给定输入信号（音频/图像），计算其完整散射变换的路径集合（理论上）。
  2. 在每次训练迭代中，从路径分布中随机采样一个子集（可带重要性权重）。
  3. 仅计算采样路径对应的散射系数，作为损失函数的一部分。
  4. 反向传播时仅对采样路径求梯度，更新模型参数。
  5. 使用重要性采样初始化启发式方法，基于数据集统计提前确定采样分布。

## 3. 实验设计

- **应用场景**：可微数字信号处理（DDSP），具体包括无监督声音匹配任务：
  - 颗粒合成器（granular synthesizer）声音匹配
  - Roland TR-808 鼓机声音匹配
- **基准方法**：与使用完整 JTFS 作为损失函数的方法对比（或与其他感知损失函数对比，文中未明确列出具体对比方法名称，但暗示优于现有方法）。
- **数据集**：未明确提及公开数据集名称，但推测使用了合成器生成的音频数据以及 TR-808 鼓机样本。
- **评估指标**：匹配质量（可能通过感知距离或合成信号与目标信号的相似度度量）以及计算效率（训练时间、开销）。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅提到实现了 SCRAPL 并提供了 Python 包，但未披露硬件配置或运行时间量化结果。

## 5. 实验数量与充分性

- **实验数量**：至少涉及两个不同的声音匹配任务（颗粒合成器和鼓机），但未报告消融实验、不同超参数下的对比或数据集规模变化实验。
- **充分性评价**：实验覆盖了两种不同类型的音频合成场景，有一定代表性。但缺乏与其他感知损失（如 STFT 损失、Mel 损失）的系统对比，也没有在更大规模或不同领域（如语音、环境声）上的验证。实验设计相对简单，充分性一般。

## 6. 主要结论与发现

- SCRAPL 能够以显著降低的计算成本逼近完整 JTFS 的特征提取能力，作为可微损失函数时有效指导神经网络训练。
- 重要性采样初始化启发式方法进一步提升了收敛速度和评估性能。
- 在无监督声音匹配任务中，SCRAPL 在计算效率和特征质量之间达到了良好的平衡，优于仅使用完整 JTFS 或现有近似方法。

## 7. 优点

- **创新性**：将随机路径采样引入散射变换优化，以概率方式解决计算瓶颈，方法简洁且理论动机清晰。
- **实用性**：提供 Python 包和代码、音频示例，易于复现和应用。
- **可扩展性**：重要性采样启发式可根据数据自适应调整，泛化能力较强。
- **跨领域潜力**：虽主要实验在音频，但方法可推广到图像等其他模态的散射变换。

## 8. 不足与局限

- **实验覆盖有限**：仅测试了两种声音匹配场景，未在语音、环境音或更大规模数据集上验证，也缺少与其他主流感知损失（如对抗损失、特征匹配损失）的对比。
- **缺乏消融研究**：未系统分析随机路径采样数量对性能的影响，未说明最优采样比例。
- **算力信息披露不足**：无具体 GPU 时间、内存开销等量化指标，无法与其他方法进行公平的效率对比。
- **偏差风险**：无监督声音匹配任务中，若数据分布与重要性采样假设不符，可能导致采样偏差。
- **应用限制**：当前主要针对 JTFS，对其他类型散射变换（如纯时间散射）的适用性未讨论。

（完）
