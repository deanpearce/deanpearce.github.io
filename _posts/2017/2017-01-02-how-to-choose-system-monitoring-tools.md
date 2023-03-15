---
title: "How to Choose System Monitoring Tools"
date: "2017-01-02"
categories: 
  - "monitoring"
  - "system-administration"
tags: 
  - "administration"
  - "monitoring"
  - "system-health"
---

There are dozens of monitoring solutions available in the marketplace. Beyond commercial tools, there are many custom, smaller, or in-house tools. For example I am the prime developer of a tool called Avail which is developed at [Pythian](https://www.pythian.com/). Since we are primarily a managed services company, this tool needs to run on every imaginable platform in existence. Beyond this, since we specialize in database and database applications, we need to be able to reliably and flexibly monitor a huge number of database products. When looking for your own monitoring needs, there are many things to consider.

#### What is your budget?

Unfortunately monitoring is often the first item to be cut when dealing with budgeting. Some managers think that monitoring doesn't contribute to the bottom-line of the company, but I would argue that they're wrong. Monitoring is a vital part of providing a service that is reliable and can be audited. Monitoring tools can assist in preventing security incidents through patch and compliance monitoring. Monitoring tools also assist in narrowing down service performance issues, providing SLA verification, and ensuring that your current infrastructure is meeting your needs. When [3 seconds of waiting decreases customer satisfaction by about 16%](https://blog.kissmetrics.com/loading-time/), it's important to ensure your service is reliable and responsive every time. I actively encourage customers to consider monitoring at development time - it's significantly easier to instrument new configurations and code than to retrofit and find funding when you're about to go live.

There are many commercial solutions available in the marketplace at many price points. There are SaaS solutions, on-premise solutions, or depending on your infrastructure, integrated solutions. Many tools offer per-CPU or per-Server licensing, but an increasing of SaaS providers are offering different billing metrics including by the hour, by the check, or by the instance. Beyond commercial, there are also many free solutions available for different infrastructure setups. There are pros and cons to each infrastructure layout and each tool, which I will explore in depth in a future article.

#### What are you trying to monitor?

Traditionally servers were owned and housed in a data center by an organization. This typically meant a fixed number of assets with total ownership. As such, beyond your applications and services, you frequently had to monitor the health of your physical systems as well as the load and health of your applications. Things like disk state, temperature, configuration, and load were critical to ensuring a high quality of service. Virtualization abstracted away some of these issues, allowing load to be balanced and services to be shifted off troublesome hosts.

Many companies are now choosing cloud solutions for their applications, be it private clouds or public cloud utilities. This changes the nature of monitoring - no longer are you managing fixed assets, you are monitoring a service as you scale with demand. Virtual servers and application instances are spun up and down on demand, and no longer are you concerned with the health of a specific physical machine or even virtual host. This dynamic nature means you need a monitoring solution that can be deployed automatically and monitor the application and service metrics that you care about. Some cloud provides such as Amazon have built out services such as CloudWatch to monitor the status of your service. But these tools tend to scale in cost with your service, and don't provide fine-grained integration that traditional monitoring tools have enjoyed. Some services like NewRelic have addressed application performance and reliability monitoring, but have high cost and high integration effort.

#### What are your monitoring metrics and goals?

Depending on the design and nature of your business there are many things to consider when it comes to monitoring. There are also three high-level metrics to consider when designing a monitoring solution for your systems: severity, risk, and preventability. Severity relates to the the impact that an outage would have on your business. Risk relates to how likely an issue is likely to occur. Preventability relates whether an issue can be prevented before it causes an outage. As a result, designing an effective monitoring solution can be extremely complex. When evaluating monitoring tools you need to consider what kind of issues can be captured, and the cost in terms of time and software customization. Some things to consider when evaluating the need and complexity of a monitoring solution:

- Is your service online and responsive?
- Are your SLAs being met in terms of backups, service availability, response times?
- Are your security needs met in terms of patching, vulnerabilities, and configuration?
- Are all key aspects of your service or system being monitored?
- How are your thresholds configured and reviewed?
- Do you have proactive monitoring in place to catch issues before they cause a failure?
- Are you monitoring all aspects of your system?
- Does the tool allow for auditing and historical analysis?

#### What is the difference between agent and agentless monitoring?

Traditionally monitoring a system meant installing an agent on a system, and having that agent configured to monitor a number of metrics, potentially with custom scripting. A popular alternative to this is agentless monitoring. A central service (SaaS or otherwise) would connect using established protocols such as SSH on Unix and WMI on Windows, and run queries and system commands against the system. The data would be parsed and managed remotely. The advantage of this approach is you would only need to configure permissions and connectivity to a system. While remote access may be ideal for configuration and deployment, the need to transfer data remotely may violate compliance with regulation such as PCI DSS or HIPAA. Other limitations may be platform dependent - you may not be able to access all application features and information on all OS platforms and security breaches could result in large data breaches.

#### What kind of integration do you need with your applications?

You may also want to consider the needs of your organization. Application instrumentation and monitoring may be an essential part of your daily operations. Tools like NewRelic can integrate with your application along with off-the-shelf software to provide request and resource-level monitoring. Tools like Nagios an be customized to integrate with your tools as well through custom scripting. Sometimes it is sufficient to monitor the system and high-level components such as the database and core services running on the system.
