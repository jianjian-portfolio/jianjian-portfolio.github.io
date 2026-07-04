---
title: "个人简介"
description: "建健（Jian Jian），NetApp 资深应用科学家——深度学习、生产级 ML、Nature 子刊论文、共同发明专利与开源项目。"
lastmod: 2026-07-04
sitemap:
  priority: 1.0
layout: single
aliases:
  - /about/
hideMeta: true
---

**现任 NetApp 资深应用科学家（Staff Applied Scientist）**。在 17 年的技术生涯里，我的研究与工程实践主要在深度学习研究、多智能体 AI 系统、数据科学以及高性能企业级系统等领域展开。我专注于设计并部署可扩展的智能系统、异常检测以及硬件推理加速，致力于将深奥的数学理论同硬核的系统工程相结合，在严苛的延迟和算力限制下，死磕生产级 AI 解决方案的工程落地。

目前，我在 NetApp 带着一支 AI & ML 研发团队，致力于把最前沿的 AI 研究真正做进企业级系统里。在此之前，我曾是 DeepCell 的数据科学家（Data Scientist），负责深度学习模型的开发与落地。我的科研经历包括在 Daniel Acuña 教授的 [Science of Science & Computational Discovery Lab](https://scienceofscience.org/)（雪城大学 iSchool，2020–2021年）担任研究员。期间，我们合作撰写了发表于 Nature 子刊的[科学资源寿命预测论文](https://www.nature.com/articles/s41599-025-04716-z)，将严谨的统计建模与海量元数据挖掘融合在了一起。

这种"数学直觉"与底层系统工程（C++、Java）的双重背景，使我能贯通机器学习生命周期的关键环节——从自定义数学算法，到基于 GPU/TPU、PyTorch、TensorFlow、TensorRT 与 ONNX 的高吞吐推理部署。关于我 17 年来从传统系统开发逐步转向应用 AI 研究的完整职业历程，欢迎在 [职业历程](/zh/timeline/) 页面中查看。在应对这种跨界转型时，通往最终的目的地就像一趟长途旅程，但旅途本身就像行驶在深夜的公路上——目之所及，始终只是眼前这一段路。所以我一直信奉"活在当下"，与其为看不清的远景焦虑，不如沉下心来，把当下的每一步走扎实；更远的风景，往往会在前行中渐次展开。

---

## 研究、专利与开源项目

以下成果从**统计建模与可复现研究**，延伸到**生产级 ML 系统与专利落地**：

### 1. 学术发表与开源成果
* **Predicting the longevity of resources shared in scientific publications**（发表于 Nature 旗下 *Humanities and Social Sciences Communications* 期刊，2025年）。[DOI: 10.1057/s41599-025-04716-z](https://www.nature.com/articles/s41599-025-04716-z)
  * *作者团队*：Daniel E. Acuña, **Jian Jian**, Tong Zeng, Lizhen Liang, Han Zhuang。
  * *资金资助*：本项目获得美国研究诚信办公室（Office of Research Integrity，项目资助编号：`ORIIR190049` 和 `ORIIR180041`）项目资助。
  * *核心开源工具*：我自主研发并开源了核心计算包 **[Tobit-EN](https://github.com/UKGANG/tobit)**。这是一个在 Python 中基于最大后验概率估计（MAP）实现的带 Elastic Net 正则化的 Tobit 截断回归模型，专门用于建模和预测科学文献中数字资源的生命周期。

### 2. 发明专利（5 项已公开，2 项申请中）
* 作为核心研发团队的关键成员，我参与研发并共同申请了多项专利技术，这些技术构成了 NetApp ONTAP 实时自主勒索软件防护（Autonomous Ransomware Protection, ARP）引擎的核心算法架构（该技术曾获 [《福布斯》(Forbes)](https://www.forbes.com/sites/stevemcdowell/2024/03/06/netapp-introduces-real-time-ransomware-detection-for-ontap/) 专题报道）：
  * **Graph Vector Variation Driven Data Corruption Detection**（美国专利公开号：[US20250245326A1](https://patents.google.com/patent/US20250245326A1/en)）
  * **Vector Variation Driven Malware Corruption Detection**（美国专利公开号：[US20250245324A1](https://patents.google.com/patent/US20250245324A1/en)）
  * **Malicious encryption detection based on byte frequency distribution**（美国专利公开号：[US20250298892A1](https://patents.google.com/patent/US20250298892A1/en)）
  * **Data Exfiltration Monitoring Using Hash Values**（美国专利公开号：[US20250330488A1](https://patents.google.com/patent/US20250330488A1/en)）
  * **Data Exfiltration Monitoring Using Semantic Queries**（美国专利公开号：[US20250330487A1](https://patents.google.com/patent/US20250330487A1/en)）
  * *Protection Graph*（申请中）
  * *Clean Room Mechanism*（申请中）

### 3. 开源研究工具
* **[ImageAnnotatorJS](https://github.com/sciosci/ImageAnnotatorJS)**：一个用于构建学术图表检验与标注系统的模块化 JavaScript 库（AMD 标准），由美国研究诚信办公室（ORI）资助。

---

## 代码之外

### 魔兽世界
{{< figure-block src="/assets/images/about/wower.png" alt="WoW Paladin" embed="left" width="240" >}}
工作之余，我同时也是个资深的《魔兽世界》玩家，曾担任过 7 年多的防骑（防护圣骑士）和公会会长。对我来说，最开心的事莫过于组织几十个来自天南海北的队友，一起开荒副本、死磕机制，在经历无数次团灭折磨后拿到首杀那一刻的狂喜。其实，比起复杂的战术组织，那些在语音频道里开过的玩笑、熬过的通宵、以及大家结下的深厚友谊，才是这段虚拟冒险里最宝贵的财富。为了艾泽拉斯，*为了联盟！*

### 羽毛球
{{< figure-block src="/assets/images/about/badminton.jpg" alt="Badminton" embed="right" width="240" >}}
我从小就喜欢打羽毛球。我特别享受羽毛球里那种极致的速度感、电光石火间的反应碰撞，还有大力扣杀时球拍击中羽毛球那一声清脆的声响。不管是工作之余的养生局，还是高强度的对抗，只要站在球场上，就能让我彻底放空、挥洒汗水，享受最纯粹的快乐。

### 社区爱心募捐
{{< figure-block src="/assets/images/leadership/ncs-frontdesk.jpg" alt="NCS Singapore" embed="left" width="320" >}}
在新加坡长期出差期间，我曾发起并组织过一次爱心募捐，帮助了一位饱受强直性脊柱炎折磨、无力负担手术费的同事筹集关节置换的费用。当时我和志愿者伙伴们一起研习了新加坡本地关于募捐的法律法规、对接后勤，最终筹到了大约 4000 新币，全额资助了他的手术和康复期的开销。这次经历让我真切地感受到，来自集体的微小善意，真的可以改变一个人的命运。

<details class="premium-details premium-details--full">
  <summary class="premium-summary"><span>查看爱心募捐明细登记表</span></summary>
  <div class="premium-content premium-content--wide">
    <img src="/assets/images/leadership/ncs-donation.png" alt="NCS 爱心募捐明细登记表" loading="lazy">
  </div>
</details>
