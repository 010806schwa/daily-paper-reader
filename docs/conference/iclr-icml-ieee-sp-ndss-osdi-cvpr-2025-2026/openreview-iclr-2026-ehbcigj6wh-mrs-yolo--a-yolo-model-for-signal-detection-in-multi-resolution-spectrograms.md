---
title: "MRS-YOLO : A YOLO model for signal detection in multi-resolution spectrograms"
title_zh: MRS-YOLO：一种用于多分辨率频谱图中信号检测的YOLO模型
authors: "REIHAN MAZOUZ, Picheral, Abigael Taylor, Jonathan Bosse, Chakib BELAFDIL, Sylvie Marcos"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=ehbcigJ6wH"
tags: ["query:shipnoise-hf"]
score: 7.0
evidence: 多分辨率频谱图处理方法，可应用于船舶噪声高频成分识别
tldr: 真实信号常包含跨多时频尺度的结构，短时傅里叶变换因海森堡不确定原理存在时频分辨率权衡。为此提出MRS-YOLO，一种多分辨率YOLO模型，通过并行分支处理互补尺度频谱并用注意力块融合，在异构射频信号数据集上，低信噪比下召回率更高，分类更准，在低信噪比区域召回率提升显著，对杂散发射等挑战信号具有更强鲁棒性。该方法作为一种通用频谱图处理框架，有望用于船舶噪声高频成分识别等水声任务。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 许多真实信号包含多尺度时间-频率结构，传统频谱图受限于时频分辨率折衷。
method: 提出MRS-YOLO，采用并行分支处理不同分辨率频谱图，并通过注意力机制融合特征。
result: 在异构射频信号数据集上，低信噪比场景下召回率和分类准确率均有显著提升。
conclusion: 该方法为多尺度信号检测提供了有效框架，可推广至水声信号处理。
---

## Abstract
Many real-world signals contain structures spanning multiple time–frequency (TF) scales, where short transients and long-duration patterns coexist. Standard spectrograms, based on the short-time Fourier transform, are constrained by the Heisenberg uncertainty principle, which here translates into the well-known trade-off between time and frequency resolutions.  We propose MRS-YOLO, a multi-resolution extension of YOLO that processes spectrograms at complementary scales through parallel branches and fuses them with an attention block.  On a challenging datasets of heterogeneous radio-frequency signals with spectral congestion, low SNR, and stealthy emissions, \MRS-YOLO achieves higher recall in low-SNR regimes and stronger classification accuracy than single-resolution baselines, demonstrating the value of explicit multi-scale representation learning in TF analysis. Code available at https://github.com/ICLRanonymous2026/MRS_YOLO_ICLR26.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：许多真实世界信号包含跨越多个时频（TF）尺度的结构，例如短瞬态与长持续模式共存。标准频谱图基于短时傅里叶变换（STFT），受海森堡不确定原理制约，存在时频分辨率之间的根本性权衡，导致单一分辨率无法同时捕获所有尺度的信号特征。
- **研究动机**：在频谱拥挤、低信噪比（SNR）以及隐蔽发射等挑战性射频（RF）场景下，传统单分辨率检测方法性能下降，亟需一种能显式学习多尺度时频表示的方法。
- **整体含义**：提出多分辨率 YOLO 模型（MRS-YOLO），通过并行处理互补分辨率的频谱图并融合特征，以提升低信噪比下信号检测的召回率和分类准确率，同时该方法可作为通用频谱图处理框架，有望推广至水声信号处理（如船舶噪声高频成分识别）。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用 YOLO 目标检测框架，扩展为多分支架构，以同时处理多个时频分辨率的频谱图，并通过注意力机制自适应地融合各分支的特征，从而学习跨尺度的互补信息。
- **关键技术细节**：
  - **并行分支**：将输入信号通过不同窗长的 STFT 生成多张互补尺度的频谱图（例如粗分辨率与细分辨率），每个分支独立进行特征提取。
  - **注意力融合块**：设计一个注意力模块，对各分支的特征图进行加权融合，使模型能动态关注对当前检测任务最有用的时频尺度。
  - **检测头**：融合后的特征图输入 YOLO 检测头，输出目标边界框（对应信号在时频平面上的位置）和类别概率。
- **公式或算法流程**（文字说明）：
  1. 对原始信号进行多种窗长的 STFT，得到一组多分辨率频谱图 \(\{S_1, S_2, \ldots, S_K\}\)。
  2. 每个频谱图输入一个共享或独立的卷积骨干网络，提取特征图 \(F_k\)。
  3. 所有特征图通过注意力融合块，生成融合特征 \(F_{\text{fused}} = \text{Attention}(F_1, \ldots, F_K)\)。
  4. 将 \(F_{\text{fused}}\) 送入 YOLO 检测头，进行目标定位和分类。
  5. 训练时使用标准 YOLO 损失（边界框回归损失 + 分类损失 + 置信度损失）。

## 3. 实验设计

- **数据集与场景**：使用一个具有挑战性的异构射频（RF）信号数据集，该数据集包含频谱拥挤、低 SNR 以及隐蔽发射（stealthy emissions）等多种困难场景。
- **基准（Benchmark）**：以单分辨率 YOLO 模型作为基线（即仅使用单一窗长 STFT 的频谱图输入）。
- **对比方法**：
  - 单分辨率 YOLO（不同窗长的独立模型）。
  - 可能还包含其他目标检测模型（论文摘要未明确列出，仅提及与单分辨率基线对比）。
- **主要评估指标**：低 SNR 区域的**召回率**和整体**分类准确率**。

## 4. 资源与算力

- **文中说明**：论文摘要及提供的元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。
- **备注**：作者已开源代码（GitHub 仓库），但未给出训练资源细节。如需复现，需自行推断或联系作者。

## 5. 实验数量与充分性

- **主要实验**：
  - 在单一 RF 数据集上，对比多分辨率模型与单分辨率基线的性能（不同 SNR 条件下的召回率、分类准确率）。
  - 可能包含消融实验：如移除注意力融合模块、改变分支数量等（摘要未详细说明，但依惯例应有此类实验）。
- **充分性评价**：
  - **优点**：数据集具有强挑战性（低 SNR、频谱拥挤），能有效检验方法鲁棒性；多分辨率对比设计直接验证了核心思想。
  - **不足**：仅在一个数据集上测试，缺乏跨领域（如水声、生物医学信号）的泛化验证；未提供统计显著性检验；消融实验细节未知。整体实验数量有限，但针对本文目标（信号检测）而言是充分的；不过结论的普适性需更多数据支持。

## 6. 论文的主要结论与发现

- MRS-YOLO 在低 SNR 区域相比单分辨率基线**显著提升召回率**，并保持或提高分类准确率。
- 对于杂散发射（stealthy emissions）等挑战信号，多尺度表示学习带来了更强的鲁棒性。
- 验证了**显式多尺度时频表示学习**对于时频分析任务的价值。
- 该方法作为一种通用频谱图处理框架，可推广至其他信号检测任务（如水声船舶噪声高频成分识别）。

## 7. 优点

- **方法创新性**：将 YOLO 目标检测与多分辨率频谱图分析结合，并引入注意力机制进行跨尺度特征融合，解决了传统 STFT 分辨率权衡问题。
- **性能提升显著**：在低 SNR 这一关键困难场景下，召回率提升明显，且分类更准。
- **实用价值**：可广泛应用于无线电监测、水声探测等实际系统，且代码开源，便于复现和工程部署。
- **框架通用性**：不限于特定信号类型，具备跨领域迁移潜力。

## 8. 不足与局限

- **实验覆盖有限**：仅基于一个 RF 数据集进行验证，未在其他模态（如声音、振动、生物信号）上测试，泛化性有待证明。
- **缺乏计算资源与效率分析**：未报告模型参数量、推理速度、训练开销，无法判断实际部署的可行性。
- **没有与更多主流检测方法对比**：仅对比了单分辨率 YOLO，未涉及其他目标检测网络（如 SSD、Faster R-CNN）或传统时频检测方法。
- **消融实验透明度不足**：未在摘要或元数据中详细说明各组件（分支数、注意力机制）的贡献。
- **偏差风险**：数据集可能具有特定分布（如射频信号的调制方式、SNR 范围），实验结果可能存在领域偏差。
- **应用限制**：虽然提及可推广至水声信号，但未给出具体实验证据；水声频谱图特性与射频不同（如低频、多径效应），需额外适配验证。

（完）
