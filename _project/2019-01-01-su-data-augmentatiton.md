---
title: "Data Augmentation for X-Ray Screening Images"
last_modified_at: 2021-08-18T04:35:02-05:00
company: Project
datetime: 2021-08-18T04:35:02-05:00
order: 13
---

The objective of this experiment is to augmenting data to increase the robustness of the AI model. I am responsible for devising the architecture of the generative adversarial network (Gan), designing the step for the experiment, as well as analyzing the performance of the model. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/gan.jpeg){: .align-center}

Firstly, I experimented on two different network architectures with extensive hyperparameter tuning. Secondly, by appending a Sobel filter, I advanced the network to capture features of the image. Also, I conducted the NHST to analyze the performance of the network. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/dcgan.gif){: .align-center}

To cope with the limitation of the GPU capacity restriction of Google Colab, I enhanced the program with Tensorflow checkpoint functionality to improve the training process in an interruptible manner. Eventually, the optimized network can provide mock X-Ray images to offer augmented data for medical researching purposes.