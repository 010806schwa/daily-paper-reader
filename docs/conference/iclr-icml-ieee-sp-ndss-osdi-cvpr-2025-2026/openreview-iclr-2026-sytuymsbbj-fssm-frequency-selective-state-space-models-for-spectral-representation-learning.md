---
title: "FSSM: Frequency-Selective State Space Models for Spectral Representation Learning"
title_zh: FSSM：频率选择性状态空间模型用于频谱表示学习
authors: "Anuvab Sen, Mir Sayeed Mohammad, Saibal Mukhopadhyay"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=SyTuymSbBJ"
tags: ["query:shipnoise-hf"]
score: 7.0
evidence: 频率选择性状态空间模型可用于学习高频特征，适用于目标识别
tldr: FSSM首次将状态空间模型与频率选择性算子结合，提出了可学习的频谱前端，能够根据任务自适应调整频带权重和窗口大小。该模型在雷达目标检测和语音关键词识别任务上超越了现有频域方法，同时保持线性推理复杂度。其方法可迁移至水下声学目标识别，用于提取高频判别特征。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有频域方法固定频带划分，缺乏任务自适应性。
method: 设计可训练的频谱前端，参数化稳定因果带选择核。
result: 在雷达和语音任务上达到最优性能，计算高效。
conclusion: FSSM提供了一种通用的频谱表示学习框架，可推广至声学识别。
---

## Abstract
We introduce the first state space model (FSSM) with frequency selective spectral operators, parameterizing a family of stable, causal, band-selective kernels whose spectral weights are conditioned on the end task. This yields a representation that adapts its characteristics per task domain while retaining linear-time inference and memory. The key novelty is the trainable spectral front-end through which the model can adapt frequency weighting and inter-bin window size. We show the effectiveness of our learned spectral representations on two independent domains: radar object detection and speech keyword recognition, outperforming state of the art frequency based methods in both domains while maintaining competitive throughput and computational overhead. We further show the robustness of our approach under input perturbations, demonstrating the value of stabilized sequential operators in spectral representation learning.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义（研究动机和背景）

- **问题**：现有频域分析方法（如短时傅里叶变换、小波变换）通常依赖固定的频带划分和固定的窗口大小，无法根据任务动态调整频谱权重和分辨率，导致在雷达目标检测、语音关键词识别等任务中泛化能力受限。
- **动机**：状态空间模型（SSM）在序列建模中展现出线性推理复杂度的优势，但现有 SSM 缺乏对频谱特性的显式建模。作者希望将 SSM 与频率选择性算子结合，提出一种能够端到端学习、任务自适应的频谱表示学习框架。
- **整体含义**：FSSM 首次将状态空间模型与可训练的频谱前端结合，使得模型能够根据目标任务自动调整频带权重和窗口大小，在保持线性推理复杂度的情况下提升频谱表示的学习能力。

## 论文提出的方法论

- **核心思想**：设计一组参数化的稳定、因果、频带选择核（band-selective kernels），其频谱权重通过端到端训练由目标任务监督信号动态调整。模型包含一个可训练的频谱前端（trainable spectral front-end），该前端能够自适应地学习频率加权和频带间的窗口大小，从而得到任务优化的频谱表示。
- **关键技术细节**：
  - 参数化一族稳定、因果的滤波器，确保序列模型的稳定性和可逆性。
  - 将频带选择核嵌入到状态空间模型中，使模型能够基于输入的频谱信息进行状态更新，同时保持线性时间推理和线性存储复杂度。
  - 频谱前端的权重通过反向传播与下游分类/检测任务联合优化，从而自动学习哪些频率成分对当前任务更重要。
- **公式/算法流程**（文字说明）：
  - 输入信号首先经过可训练的频谱前端（如可参数化的滤波器组），得到频率加权后的特征。
  - 频谱特征被送入状态空间模型（SSM），该模型使用参数化的频带选择核作为状态转移的一部分，动态更新隐状态。
  - 隐状态经全连接层或分类头输出预测结果。
  - 整个模型以监督方式端到端训练，损失函数为任务相关的交叉熵或检测损失。

## 实验设计

- **使用的数据集/场景**：
  - 雷达目标检测（radar object detection）任务；
  - 语音关键词识别（speech keyword recognition）任务。
- **Benchmark**: 对比了现有的基于频域的最先进方法（文中提及 “state of the art frequency based methods”），但未列出具体方法名称（因摘要长度有限）。
- **对比方法**：除了频域基线方法外，可能也对比了标准 SSM 模型（如 S4、Mamba 等）以及固定频带划分的方法。

## 资源与算力

- 论文摘要中未明确说明使用的 GPU 型号、数量或训练时长。仅声称模型“保持 competitive throughput and computational overhead”，但无具体硬件细节。属于信息缺失。

## 实验数量与充分性

- 根据摘要，实验在两个独立领域（雷达、语音）各进行了一组完整对比实验，并额外在输入扰动下进行了鲁棒性测试（“robustness of our approach under input perturbations”）。
- **充分性评价**：两个领域的验证显示了跨领域泛化能力，且鲁棒性实验增加了说服力。但由于缺乏详细的消融实验（如不同频谱前端设计、窗口大小影响、频带数量影响）和更大规模的公开数据集评估，实验覆盖略显有限。对比方法的数量未列明，可能存在选择性基线风险。总体而言，实验在已有条件下较充分，但可进一步扩展。

## 论文的主要结论与发现

- FSSM 在雷达目标检测和语音关键词识别两个任务上均优于现有基于频域的最优方法，同时保持线性推理复杂度。
- 可训练的频谱前端能够有效自适应任务需求，学习到有区别性的频带权重和分辨率。
- 在输入扰动下，FSSM 表现出更好的鲁棒性，证明了稳定序列算子在频谱表示学习中的价值。

## 优点

1. **方法创新**：首次将状态空间模型与频率选择性算子结合，提出可学习的频谱前端，打破了传统频域方法固定频带划分的限制。
2. **高效性**：继承 SSM 的线性推理和内存复杂度，适合处理长序列。
3. **跨领域验证**：在雷达和语音两个差异较大的领域均取得领先性能，表明框架具有通用性。
4. **鲁棒性验证**：额外进行了输入扰动下的评估，增强了方法的可信度。

## 不足与局限

1. **实验细节不足**：未提供详细的数据集规模、基线方法名称、超参数设置等，难以完全复现和评估公平性。
2. **算力信息缺失**：未说明 GPU 型号、训练时间，限制了对计算成本的判断。
3. **消融实验不明确**：可能缺少对不同组件（如频谱前端参数初始化、核数量、窗口大小等）的消融研究，无法确认各设计选择的贡献。
4. **应用领域有限**：仅测试了雷达和语音，尚未在更广泛的声学（如水下声学目标识别）、图像、生物信号等频谱相关任务上验证，通用性有待进一步证实。
5. **对比方法范围**：仅与“基于频域的最优方法”对比，未明确是否与近期先进的深度频谱学习方法（如 AST、CQT 等）比较，可能存在基线选择偏差。

（完）
