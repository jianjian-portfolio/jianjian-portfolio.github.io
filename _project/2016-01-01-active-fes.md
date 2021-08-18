---
title: "Foundation Email& File Storage Service"
last_modified_at: 2021-08-18T04:27:02-05:00
company: Project
datetime: 2021-08-18T04:27:02-05:00
order: 9
---

This project is the infrastructure of the enterprise. It organizes the file storage and emails delivery service for other applications.  

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/active.jpg)
{: .full}

As a Java software engineer, I selected the candidate technologies, designed the architecture of the project, developed the project code, and evaluated the performance of this application. I also mentored and reviewed the junior's work to make sure everything was on track. Before refactoring the whole project, the legacy system was cumbersome that cannot be maintained. So, the other applications had to adapt to this service. In addition, there are many unnecessary direct MQ invocations in the system. To break this bottleneck, I first evaluated the business impact of the potential changes. Then centralized the Twilio Sms sending service and the Sparkpost API for phase 1. And I scheduled a periodical cleaner to purge the legacy file stored in the NFS space. I used the design pattern - chain of responsibility to reorganize the architecture. After the reconstruction, I downloaded the memory snapshot through the JMS interface. With the help of the memory analyzer, I identified the bottleneck of the system. After applying the new configuration, the performance of this application can handle requests 3 times faster than before. And feedback from other projects suggests that the API is much easier to use. In return, the department can keep up with the latest business requirement.
