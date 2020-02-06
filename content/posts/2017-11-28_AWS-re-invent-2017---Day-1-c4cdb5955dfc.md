---
title: 'AWS re:invent 2017 — Day 1'
description: >-
  The focus of my first day at Reinvent was mostly on the topic of security and
  financial services. Re:invent is very crowded and there’s…
date: '2017-11-28T03:50:56.264Z'
categories:
- Conference
author: "Kasper Nissen"
type: "post"
image: /images/1__TjRW94gHYjs__palVBqkjQg.jpeg
---

The focus of my first day at Reinvent was mostly on the topic of security and financial services. Re:invent is very crowded and there’s crazy lines for some of the sessions. Note to self: If you don’t have a reserved seat — come early (like really early). In the following I will give a short write up of the breakout sessions that I attended.

All right, let’s get started.

#### Security Anti-Patterns: Mistakes to Avoid

![](/images/1__TjRW94gHYjs__palVBqkjQg.jpeg)

The first talk of the day featured Kurt Gray, Global Solutions Architect for Financial Services at AWS, and Jonathan Baulch, Director, Architecture at Fidelity Investments.

Kurt started out by defining a anti-patterns, with the following great slide.

![](/images/1__m2kcg__p7IScCIdxvrDptnQ.jpeg)

Next, he presented a bunch of security anti-patterns. In the following, I will try to highlight some of these and present the takeaways from this presentation.

**_Personally Owned AWS Accounts  
_**Don’t sign your accounts up on individual people, what happens if they don’t show up some day? Instead use group distribution lists for emails, etc. Further, use MFA devices that everybody can get their hands on.

**_AWS Account Overcrowding  
_**Don’t overcrowd your AWS account. It makes auditing a lot harder, further, the blast radius will be a lot bigger if compromised.

![](/images/1__QcZoBsmZEniiDwqy7GqT9A.jpeg)

Instead, use a multi-account strategy. It will limit exposure if something gets compromised. Think of your AWS accounts as single family homes

![](/images/1__c9rCVwu9T12mkCz6JlF8YQ.jpeg)

**_Trusted IP Access w/o Client Auth  
_**Routing is not security. Implement and use a proper authentication methods instead.

**_Network Egress Backhauling  
_**Limit access to resources instances can access by filtering traffic. An example of this is to use an exit VPC to restrict egress.

**_Security Questionnaires  
_**Instead of using home-brewed questionaries, Use standardized controls, e.g., ISO certifications, etc.

**_Manual Technical Auditing  
_**How are you auditing yourself? Stop using manual steps, instead automate this process using Continuous Automated Auditing.

![](/images/1__qNDNRcQTbUxfPKgqeig6ow.jpeg)

**_Over-the-wall Software Delivery method_**   
Over-the-wall software delivery method is where dev, QA, and ops are kept separate and software is handed off between departments. Instead, you should move towards a DevOps model with small interdisciplinary delivery teams and where developers are on-call. You built it, you run it.  
Add Security and enter DevSecOps. Proactive security checking, penetration tests etc.

![](/images/1__XBpWdEbrn2mqU7bl5mkBfw.jpeg)

After the presentation by Kurt, Jonathan from Fidelity Investments took over and talked about how they do DevSecOps at Fidelity Investments.

He talked about the three different layers of security; Prevention, Detection, and Remediation. Further, he introduced the command line tool cfn\_nag, a linting tool for CloudFormation Templates that will help you catch holes in the pipeline before reaching production.

**_Key-takeaways  
_Standard Controls**: Prescriptive. Certifiable.  
**Managed Services**: Consistent Controls, Less overhead  
**DevSecOps practices**: Faster Delivery, Faster Patching, Faster Innovation.

#### Culture Shift: How to Move to Global Financial Services Organization to a DevOps Operating Model

The second talk of the day featured Alan Garver, Mahdi Sajjadpour, and Jonathan Sywulak and focused on how to move a global financial service organization to a DevOps operating model. Alan started out by listing different categories of challenges

*   Tooling challenges
*   Organization challenges
*   Financial enterprises challenges

Back in the 2000s, Amazon was built as a monolithic application. They made the transition to a service-oriented architecture and shifted two the 2 pizza teams organization. These teams should only be responsible for their product.

![](/images/1__OlyNVAloIUGuQ270Nb8WUQ.jpeg)

Some key elements in this transformation includes

*   Move from manual handoffs to “as a service”
*   Automate all the things
*   Simplify and decompose monoliths

Further, this transformation also included incorporating infrastructure code relevant to a specific service to live alongside the source code.

**_Enabling self-service at scale_**

How do you then enable all the necessary tools for enabling developers to be self-driving and autonomous while still being compliant and secure?

![](/images/1__Z28x5Z8biZiHR2ZcM__KJeg.jpeg)

**_What does self-service infrastructure enable?_**

*   Faster innovation
*   Repeatable
*   Scalable
*   SecureLeast privilege
*   Testable
*   Immutable

**_How to get to self-service?_**

![](/images/1__s__QQIxwA__9B77VwMh__pV8A.jpeg)

We need to make sure that developers can get the resources they need.

One way to enable self-service while keeping everything tight is to use Cloud Formation Templates for making things accessible in the AWS Service Catalog (standardized patterns, pre-approved)

The presentation further included a demo of how to build a unified interface for providing developers with the tools they need while still being conformant with company policies. Again the cfn\_nag tool was highlighted as a great way for ensuring policies for Cloud Formation templates.

To summarize this talk; you can provide your developers (your customers) with the tools they need in order to fast and easily deploy their features, while still enforcing policies.

![](/images/1__wZgF2HSRkdZhjM1wEMb7rg.jpeg)

#### Getting Started with Amazon Aurora

The third talk of the day was an introduction to Amazon Aurora. I’ve been following the recent news of the Postgres compatibility and was very excited to learn more. This presentation featured Gurmit Ghatore, Brandon O’Brien, and Debanjan Saha.

**_What is Aurora?_**

![](/images/1__gB__yxbOGuZfRI15MBO__3Sw.jpeg)

Aurora is a fairly new database offering from AWS and can be a drop-in replacement for MySQL and Postgres. It promises a lot! Better performance, higher availability, and cheaper. And all this is delivered as a managed services on AWS.

The more technical side of things includes a scale-out and distributed design, built as a service oriented architecture, which will automate a lot of administrative tasks. Aurora automatically replicas over 3 Availability zones and always keeps 6 copies in order to tolerate zone failures.

![](/images/1__vGq__14XNWDXx3QyZx35RbQ.jpeg)

It integrates with many of AWS other cloud services such as S3, lambda, IAM, etc. And as it is a drop-in replacement for MySQL and Postgres — all third-party tools should be compatible as well.

In terms of performance compared to Postgres and MySQL, they promise a 3x improvement over Postgres and 5x compared to MySQL.

AWS Aurora customers have migrated to Aurora from Cassandra and have seen significant savings in cost and management.

**_How did they achieve this?_**

![](/images/1__m3kMbVDHLUECu6iYMXV4hg.jpeg)

Basically, they are doing less and have made some significant improvements in terms of processing work asynchronously.

The last part of this presentation included a story of how Expedia had migrated from a setup that included Cassandra to using Aurora instead.   
They saw cost savings vs. Cassandra at 10–15% and minimized the management task of keeping everything up significant. Aurora further lets them scale very easy by adding more read replicas.

#### Born in the AWS Cloud: How Eagle Genomics Uses AWS to Process Billions of DNA Sequence Reads

The last talk of the day featured Raminderpal Singh and Nick James. This presentation focused on how they had built a pipeline for processing billions of workflows.

They leveraged a tool called eHive and explained further how they had moved to Docker and Docker Swarm to orchestrate the processing of the workflows.

I must admit, I was a bit tired at the end and the write up of this presentation is fairly short.

Great first day at Re:invent even though it didn’t include much talk about containers nor Kubernetes. I learned a ton — and will definitely go back home and investigate and try out AWS Aurora. It really sounds like a no-brainer if you are already using AWS RDS Postgres or MySQL.