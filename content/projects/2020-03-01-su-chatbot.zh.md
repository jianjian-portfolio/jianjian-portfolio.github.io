---
title: "COVID-19 智能问答机器人"
description: "COVID-19 BERT 问答机器人：Quora 语料微调 + Dash 门户，提供公众问答并收集研究问题。"
date: 2020-03-01T00:00:00Z
featured: false
cover:
    image: "/assets/images/projects/chatbot.png"
    style: "screenshot"
    alt: "Project Cover"
    relative: false
---

该问答机器人项目涉及开发自然语言处理 (NLP) 模型，向公众提供有关 COVID-19 的最新动态。担任团队负责人期间，负责主持每周的 POC 演示、指导团队分解任务、跟踪项目进度并监督代码实现。

为了构建该应用，我们首先需要收集数据。我们构建了一个网络爬虫，用于从 Quora 社区收集问句数据，接着使用预训练模型 BERT 进行迁移学习，在爬取的语料集上训练我们的 NLP 模型。在研究了用于该 NLP 任务的几种损失函数后，我们决定使用余弦相似度，并结合 ETL 流程与基于 KNN 算法的相似度比对逻辑来实现。

为了让用户能方便地使用问答机器人，我们使用 Python Dash App 框架创建了一个 Web 门户网站。该应用不仅为公众提供疫情相关信息，同时收集用户提出的问题以用于社会科学研究。为了优化模型性能，我们尝试了多种方法，如微调 BERT 的超参数、优化序列长度和采用不同的批次大小。最终产品是一个能够向用户提供最新 COVID-19 资讯并收集学术研究问句的智能问答机器人。
