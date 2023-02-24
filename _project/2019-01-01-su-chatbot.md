---
title: "Covid-19 Chatbot"
last_modified_at: 2021-08-18T04:32:02-05:00
company: Project
datetime: 2021-08-18T04:32:02-05:00
---

This chatbot project involved developing a natural language processing (NLP) model to provide the latest updates on COVID-19 to the general public. As the team leader, my responsibilities included hosting weekly proof-of-concept (POC) demonstrations, guiding the team to break down tasks, tracking project progress, and overseeing code implementation.  

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/projects/chatbot.png){: .align-center}

To build the application, we first had to collect data since we didn't have any on hand. We built a web scraper to collect data from the Quora community, and then we used transfer learning with the pre-trained model BERT to train our NLP model on the scraped dataset. After researching several loss functions for the NLP task, we decided to use cosine similarity, which we implemented using the ETL process and the logic of the similarity comparison based on the KNN algorithm.

To give users access to the chatbot, we created a web portal using the Python Dash app. This application offered COVID-related information to the general public while collecting user questions for research in social sciences. To optimize the model's performance, we experimented with various approaches such as using a Sobel filter to capture features of the image, and we also enhanced the program with TensorFlow checkpoint functionality to improve the training process in an interruptible manner. The final product was an effective chatbot that provided users with up-to-date COVID-19 information while also gathering questions for research purposes.
