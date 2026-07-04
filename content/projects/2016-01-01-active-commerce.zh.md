---
title: "Active 商业引擎架构"
description: "Active Network 商户电商 API：支付重构、令牌桶限流与 MongoDB 变更流，系统吞吐达 300 TPS。"
date: 2016-01-01T00:00:00Z
lastmod: 2026-07-04
featured: false
cover:
    image: "/assets/images/projects/active.jpg"
    style: "logo"
    logoWide: true
    alt: "Active 电商引擎"
    relative: false
---

该电商项目将商户 API 无缝集成至平台，为其他应用提供报价与下单服务。担任高级 Java 工程师期间，负责开发新功能以满足业务团队需求，并支撑项目的快速增长。

为改善用户体验，重新设计了 API，使支付服务更加便捷；并带领两人小组实现追踪日志（tracking log）模块，大幅简化了故障排查流程。此外，团队还引入了一套子系统，供业务分析团队开展 A/B 测试。

针对第三方服务回传背压（backpressure）信号的问题，提出并实现了令牌桶（token bucket）限流算法；为进一步优化系统性能，将 MongoDB Streaming 与函数式编程相结合，将系统吞吐量提升至 **300 TPS**。

项目还首次在生产环境中落地统计分析能力，进一步提升了系统整体性能；上述工作对该快速演进项目的成功起到了关键作用。
