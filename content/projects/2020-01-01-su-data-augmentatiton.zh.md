---
title: "基于 GAN 的 X 射线胸片图像数据增强"
description: "雪城大学 GAN X 射线筛查影像数据增强研究，含 Sobel 滤波与 NHST 统计验证。"
date: 2020-01-01T00:00:00Z
lastmod: 2026-07-04
featured: false
cover:
    image: "/assets/images/projects/gan.jpeg"
    style: "screenshot"
    alt: "Project Cover"
    relative: false
    hiddenInSingle: true
---

该实验旨在通过针对 X 射线影像的数据增强，提高 AI 模型的鲁棒性。担任项目负责人期间，负责设计生成对抗网络 (GAN) 的架构、规划实验步骤以及分析模型性能。

{{< figure-stack embed="left" >}}
{{< figure-block src="/assets/images/projects/gan.jpeg" alt="GAN 架构示意图" caption="生成对抗网络系统" stacked="true" width="480" >}}
{{< figure-block src="/assets/images/projects/dcgan.gif" alt="DCGAN 训练演进" caption="生成图像质量演进过程" stacked="true" width="288" >}}
{{< /figure-stack >}}

首先尝试了两种不同的网络架构，通过广泛的超参数微调来优化 GAN 性能。此外，为捕获 X 射线图像的特征，通过附加 Sobel 滤波器对网络进行了改进。最后，采用非参数假设检验 (NHST) 分析网络性能，证实了该方法的有效性。

考虑到 Google Colab 上 GPU 显存及运行时间的限制，使用 TensorFlow 检查点 (checkpoint) 功能对程序进行了增强，以支持可中断、断点续传的训练过程。这使得优化后的网络能够生成逼真的合成 X 射线图像，作为医学研究中数据增强的样本。总体而言，该项目展示了在增强 X 射线筛查图像以提高 AI 模型性能方面的有效方法。
