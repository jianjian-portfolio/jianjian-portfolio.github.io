---
title: "Covid-19 Chatbot"
description: "COVID-19 BERT chatbot with Quora corpus fine-tuning and a Dash portal for public Q&A and research question collection."
date: 2020-03-01T00:00:00Z
lastmod: 2026-07-04
featured: false
cover:
    image: "/assets/images/projects/chatbot.png"
    style: "screenshot"
    alt: "Project Cover"
    relative: false
---


This chatbot project involved developing a natural language processing (NLP) model to provide the latest updates on COVID-19 to the general public. As the team leader, my responsibilities included hosting weekly proof-of-concept (POC) demonstrations, guiding the team to break down tasks, tracking project progress, and overseeing code implementation.  

To build the application, we first had to collect data since we didn't have any on hand. We built a web scraper to collect data from the Quora community, and then we used transfer learning with the pre-trained model BERT to train our NLP model on the scraped dataset. After researching several loss functions for the NLP task, we decided to use cosine similarity, which we implemented using the ETL process and the logic of the similarity comparison based on the KNN algorithm.

To give users access to the chatbot, we created a web portal using the Python Dash app. This application offered COVID-related information to the general public while collecting user questions for research in social sciences. To optimize the model's performance, we experimented with various approaches such as fine-tuning BERT's hyperparameters, optimizing sequence length, and utilizing different batch sizes. The final product was an effective chatbot that provided users with up-to-date COVID-19 information while also gathering questions for research purposes.
