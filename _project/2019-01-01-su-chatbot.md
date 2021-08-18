---
title: "Covid-19 Chatbot"
last_modified_at: 2021-08-18T04:32:02-05:00
company: Project
datetime: 2021-08-18T04:32:02-05:00
order: 12
---

This chatbot is designed to offer the latest updates to the general public. I led a team of 4 to develop the NLP model for this application.  

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/chatbot.png)
{: .full}

We are given a limited amount of time to build this application. Initially, we do not have any data on hand. So, I hosted weekly based POC demonstration, guided the team to breakdown the task, tracked the progress of the project, as well as the code implementation. To make the project rolling, we built a web scraper to collect the data from the Quora community. And in the Poc stage, we borrowed the pre-trained model BERT to do the transfer learning using the scraped dataset. After a few rounds of research, we decided to use cosine similarity, because it outperformed the other loss function in the NLP task. We implemented the ETL and the logic of the similarity comparison based on the Knn algorithm. Using the python dash app, we have created a web portal to give the user access to the chatbot. This application offered the covid related information to the general public. Meanwhile, it collected the questions from the user and offered it to the research of the other social science
