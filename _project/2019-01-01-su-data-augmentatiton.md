---
title: "Data Augmentation for X-Ray Screening Images"
last_modified_at: 2021-08-18T04:35:02-05:00
company: Project
datetime: 2021-08-18T04:35:02-05:00
---

The objective of this experiment was to increase the robustness of an AI model through data augmentation for X-ray screening images. As the project lead, I was responsible for devising the architecture of the generative adversarial network (GAN) and designing the experimental steps, as well as analyzing the performance of the model. 

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/projects/gan.jpeg){: .align-center}
<p style="text-align: center; font-size: 16px"><i>The adversarial system</i></p>
To start, I experimented with two different network architectures, using extensive hyperparameter tuning to fine-tune the GAN for optimal performance. Additionally, to capture the features of the X-ray images, I advanced the network by appending a Sobel filter. Finally, I conducted a non-parametric hypothesis test (NHST) to analyze the performance of the network, which demonstrated the effectiveness of our approach. 

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/projects/dcgan.gif){: .align-center}
<p style="text-align: center; font-size: 16px"><i>The evolution of the image quality</i></p>
Given the limitations of GPU capacity restrictions on Google Colab, I enhanced the program with Tensorflow checkpoint functionality to improve the training process in an interruptible manner. This allowed the optimized network to provide mock X-ray images, which can be used as augmented data for medical research purposes. Overall, this project demonstrated the effectiveness of our approach in augmenting X-ray screening images for improved performance of AI models.