---
title: "Foundation Email& File Storage Service"
last_modified_at: 2021-08-18T04:27:02-05:00
company: Project
datetime: 2021-08-18T04:27:02-05:00
---

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/projects/active.jpg){: .align-center}{: .align-left}{: style="height: 80px"}

This project serves as the backbone of the enterprise by organizing file storage and email delivery for other applications. As the senior Java software engineer, I played a pivotal role in selecting candidate technologies, designing the project's architecture, developing the code, and overseeing the work of junior colleagues.

Before refactoring the entire project, the legacy system was outdated and difficult to maintain, which required other applications to adapt to its limitations. I identified and addressed bottlenecks in the system, such as direct MQ invocations and an excessive amount of unnecessary data stored in the NFS space.

To streamline the system, I centralized the Twilio SMS sending service and the Sparkpost API during phase 1, while scheduling regular cleanups to purge outdated files. Using the chain of responsibility design pattern, I reorganized the architecture and used JMS interface to analyze memory snapshots.

As a result of these efforts, the system can now handle requests three times faster than before, and feedback from other projects indicates that the API is easier to use. These improvements allow the department to keep up with the latest business requirements.
