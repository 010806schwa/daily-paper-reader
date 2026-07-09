---
title: Multi-Representation Attention Framework for Underwater Bioacoustic Denoising and Recognition
title_zh: 水下生物声学去噪与识别的多表示注意力框架
authors: "Amine Razig, Soulaymani Youssef, Loubna Benabbou, Pierre Cauchy"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=TcyBURFQSq"
tags: ["query:shipnoise-hf"]
score: 7.0
evidence: 水下生物声学识别涉及高频超声点击，其多表示注意力方法可迁移至舰船噪声识别
tldr: 该论文针对圣劳伦斯河口海洋哺乳动物监测中叫声覆盖低频到超声频段且混叠在环境噪声中的挑战，提出了一种多表示注意力引导框架。该方法先对频谱图进行分割生成生物相关能量的软掩膜，再将掩膜与原始输入融合进行多频段去噪分类。在实际录音数据上展示了优异的去噪和识别性能。其技术可推广至船舶辐射噪声的高频识别任务。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 海洋哺乳动物叫声频段宽且常受环境噪声干扰，现有方法难以准确识别。
method: 提出多表示注意力框架，通过频谱图分割生成软掩膜并与原始输入融合，实现多频段去噪分类。
result: 在真实录音数据上验证了框架的有效性，显著提升了去噪和分类性能。
conclusion: 该框架为水下声学识别提供了可迁移的高频特征处理方法。
---

## Abstract
Automated monitoring of marine mammals in the St. Lawrence Estuary faces extreme challenges: calls span low-frequency moans to ultrasonic clicks, often overlap, and are embedded in variable anthropogenic and environmental noise. We introduce a multi-representation, attention-guided framework that first segments spectrograms to generate soft masks of biologically relevant energy and then fuses these masks with the raw inputs for multi-band, denoised classification. Image and mask embeddings are integrated via mid-level fusion, enabling the model to focus on salient spectrogram regions while preserving global context. Using real-world recordings from the Saguenay–St. Lawrence Marine Park Research Station in Canada, we demonstrate that segmentation-driven attention and mid-level fusion improve signal discrimination, reduce false positive detections, and produce reliable representations for operational marine mammal monitoring across diverse environmental conditions and signal-to-noise ratios. Beyond in-distribution evaluation, we further assess the generalization of Mask-Guided Classification (MGC) under distributional shifts by testing on spectrograms generated with alternative acoustic transformations. While high-capacity baseline models lose accuracy in this Out-of-distribution (OOD) setting, MGC maintains stable performance, with even simple fusion mechanisms (gated, concat) achieving comparable results across distributions. This robustness highlights the capacity of MGC to learn transferable representations rather than overfitting to a specific transformation, thereby reinforcing its suitability for large-scale, real-world biodiversity monitoring. We show that in all experimental settings, the MGC framework consistently outperforms baseline architectures, yielding substantial gains in accuracy on both in-distribution and OOD data.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：圣劳伦斯河口海洋哺乳动物的自动监测面临极端挑战：叫声频率范围极广（从低频呻吟到高频超声点击），常常重叠，并且混杂在多变的人类活动噪声和环境噪声中。
- **核心问题**：现有方法难以在强噪声背景下准确识别多种频段的生物声学信号，尤其是高频成分易被掩盖或误判。
- **整体含义**：该研究旨在提出一种能够同时处理宽频段、抗噪声干扰的水下声学去噪与识别框架，以提升海洋生物多样性监测的可靠性和可扩展性。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：采用多表示注意力引导框架，首先对频谱图进行分割，生成生物相关能量的软掩膜，然后将掩膜与原始输入融合，实现多频段去噪分类。
- **关键技术细节**：
  - **频谱图分割**：将输入的声学频谱图通过分割模型生成软掩膜，强调具有生物声学能量的区域。
  - **多表示融合**：将图像（原始频谱图）嵌入和掩膜嵌入通过**中层融合（mid-level fusion）** 集成，使模型既能关注显著频谱区域，又能保留全局上下文。
  - **分类**：融合后的特征用于去噪后的多频段分类，从而提升信号判别力并降低假阳性。
- **算法流程（文字说明）**：
  1. 输入原始录音的时频谱图。
  2. 利用分割网络生成软掩膜（概率图，表示每个时频单元属于生物声学信号的可能性）。
  3. 将原始频谱图嵌入与掩膜嵌入进行中层融合（融合机制可包括门控、拼接等简单方式）。
  4. 将融合结果输入分类器，输出不同物种或叫声类型的概率。

### 3. 实验设计
- **数据集**：使用加拿大萨格奈–圣劳伦斯海洋公园研究站的真实录音数据，包含多种环境条件和信噪比。
- **测评场景**：
  - **分布内（in-distribution）**：在相同采集条件下评估。
  - **分布外（out-of-distribution, OOD）**：使用替代声学变换生成的频谱图测试，以模拟数据分布偏移。
- **基准方法**：对比了高容量的基线模型（未明确具体模型名）。
- **对比方法**：仅提到“baseline architectures”，未详细列出。此外，简单融合机制（gated, concat）被作为消融对比。

### 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及GPU型号、数量、训练时长等具体算力信息。仅有框架描述和实验结果，没有资源消耗数据。

### 5. 实验数量与充分性
- **实验数量**：摘要中主要报告了分布内和分布外两大类实验，并且提到了在多种环境条件和信噪比下测试，以及不同融合机制（门控、拼接）的对比。具体组数未列出，但涵盖了主要场景。
- **充分性与客观性**：
  - 优点：考虑到了分布外泛化，并使用了真实世界录音，有一定的实际意义。
  - 不足：缺乏详细的消融实验（如不同掩膜生成方式、不同融合层选择的对比）、统计显著性检验、以及与其他流行水下声学方法的全面对比。仅与“高容量基线”对比，可能不够全面。未公开代码或数据集细节，难以完全复现。

### 6. 论文的主要结论与发现
- 所提出的**掩膜引导分类（MGC）框架**在所有实验设置中均优于基线架构。
- 在分布内数据上，MGC显著提升了信号判别力，降低了假阳性。
- 在分布外数据上，高容量基线模型性能下降，而MGC保持稳定，甚至简单的融合机制（门控、拼接）也能在不同分布下获得相当的结果。
- 这表明MGC学习了**可迁移的表征**，而非过拟合特定变换，因此适用于大规模、真实世界的生物多样性监测。

### 7. 优点
- **方法创新**：将频谱图分割与注意力引导的掩膜融合相结合，实现多频段去噪与分类一体化。
- **鲁棒性**：在分布外泛化方面表现优异，展示了较强的适应能力。
- **实用性**：直接在真实录音数据上验证，可部署于实际海洋监测系统。
- **简洁有效**：即使使用简单的融合机制（门控、拼接）也能达到稳定性能，降低了复杂度。

### 8. 不足与局限
- **缺乏资源细节**：未报告训练成本（GPU、时间），不利于资源受限场景的评估。
- **实验覆盖有限**：
  - 数据集仅来自一个特定地点（圣劳伦斯河口），可能不适用于其他海域或不同物种。
  - 未与多种现有水下声学去噪/识别方法（如CNN、Transformer-based、WaveNet等）进行全面对比。
  - 消融实验不完整，例如未单独分析分割模块的影响、不同掩膜融合策略的详细对比。
- **偏差风险**：仅使用了一种变换生成OOD数据，可能无法代表真实世界多样的分布偏移。
- **应用限制**：方法依赖频谱图分割，可能对极低信噪比或非稳态噪声的处理效果有限；文中未讨论计算实时性要求。

（完）
