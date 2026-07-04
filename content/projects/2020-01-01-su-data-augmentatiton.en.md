---
title: "Data Augmentation for X-Ray Screening Images"
description: "GAN-based data augmentation for X-ray screening images at Syracuse University, with Sobel filtering and NHST statistical validation."
date: 2020-01-01T00:00:00Z
featured: false
cover:
    image: "/assets/images/projects/gan.jpeg"
    style: "screenshot"
    alt: "Project Cover"
    relative: false
    hiddenInSingle: true
---

The objective of this experiment was to increase the robustness of an AI model through data augmentation for X-ray screening images. As the project lead, I was responsible for devising the architecture of the generative adversarial network (GAN) and designing the experimental steps, as well as analyzing the performance of the model.

{{< figure-stack embed="left" >}}
{{< figure-block src="/assets/images/projects/gan.jpeg" alt="GAN architecture diagram" caption="The adversarial system" stacked="true" width="480" >}}
{{< figure-block src="/assets/images/projects/dcgan.gif" alt="DCGAN training evolution" caption="The evolution of the image quality" stacked="true" width="288" >}}
{{< /figure-stack >}}

To start, I experimented with two different network architectures, using extensive hyperparameter tuning to fine-tune the GAN for optimal performance. Additionally, to capture the features of the X-ray images, I advanced the network by appending a Sobel filter. Finally, I conducted a non-parametric hypothesis test (NHST) to analyze the performance of the network, which demonstrated the effectiveness of our approach.

Given the limitations of GPU capacity restrictions on Google Colab, I enhanced the program with Tensorflow checkpoint functionality to improve the training process in an interruptible manner. This allowed the optimized network to provide mock X-ray images, which can be used as augmented data for medical research purposes. Overall, this project demonstrated the effectiveness of our approach in augmenting X-ray screening images for improved performance of AI models.
