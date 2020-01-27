---
title: ContainerDays Hamburg
description: >-
  The first European ContainerDays was held in Hamburg (Germany) on June 27 and
  28, 2016. The first day consisted of workshops about…
date: '2016-07-13T18:53:15.931Z'
categories: []
keywords: []
slug: /@phennex/containerdays-hamburg-c4639abae4fa
---

The first European [ContainerDays](http://www.containerdays.de/) was held in Hamburg (Germany) on June 27 and 28, 2016. The first day consisted of workshops about Kubernetes, Docker (security), DC/OS, rkt, and monitoring. The second day consisted of presentation sessions held by 16 different speakers. We went on a trip from Aarhus to Hamburg to check out the second day’s sessions.

![](img/0__sew5kO0JrAGDZ9fH.jpg)

**From Borg to Kubernetes: The History and Future of Container Orchestration** — _Mandy Waite, Senior Staff Developer Advocate, Google Cloud Platform._ Mandy described the evolution of Google’s internal cluster management systems, especially focusing on how the different systems work and resource scheduling. Internal experiments and lessons learned with Borg was presented in a condensed format. The high server utilization Google has achieved was presented, and among the important factors are bin packing and overcommitment of resources. The details behind Google’s internal experiments and lessons can be found in the Google white paper: [Large-scale cluster management at Google with Borg](http://research.google.com/pubs/pub43438.html). Furthermore, the evolution of different systems: Borg, Omega, and the open-source Kubernetes was described. Further details about this topic can be found in the Google white paper: [Borg, Omega, and Kubernetes](http://research.google.com/pubs/pub44843.html).

![](img/0__0Ua27Vo3gZ5UXcXr.jpg)

**Enterprise Microservices Adoption** — _Boyan Dimitrov, Sixt._ One of the key takeaways from Boyan’s talk; ‘always do reality checks’. A great illustration of this is, engineers saying: “We need to build race cars” (which is the new cool thing of the week). When they finished building these new race cars, they realize they don’t have a track to drive the car — and they are forced to either throw away this awesome new car or build a brand new track. Therefore, always ask yourself, are we building the right thing? Boyan further described how they have adopted microservices and Continuous Delivery at Sixt. Technologies they used: HuBot, Jenkins, BitBucket, Docker, and Kubernetes.

![](img/0____91tYniMhto8nK7I.jpg)

**Linux kernel features building my $CONTAINER** — _Erkan Yanar, Freelancer._ The focus of this talk was the deeper understanding of the Linux kernel features that container technology like Docker leverage. Erkan demonstrated how to use chroot, cgroups, and namespaces while describing what they make possible from a conceptional view. A fast-paced, entertaining, and educational tour of the underlying concepts behind Docker and Pods in Kubernetes.

**Application Deployment and Management at Scale with 1&1** — _Rainer Sträter, ..1&1._ 1&1 are launching a new platform for hosted solutions built on Kubernetes. Rainer gave a thorough description of the advantages of moving from their own model following old Cloud Hosting principles.

**Rancher Docker — From zero to hero** — _Michael Vogeler, Nexinto Gmbh._ Unfortunately, we missed some of this talk, but it was a demonstration of how to setup a Kubernetes cluster using Rancher. It seemed really interesting, especially with the features of easily deploying a cluster as a ‘one click’ install whether it was a Docker Swarm, Kubernetes or Mesos cluster.

![](img/0__7j6BtiLKcF7kIos2.jpg)

**Plan B: Service to Service Authentication with OAuth** — _Henning Jacobs, Zalando._ Henning presented Zalando’s investigation of authentication between microservices and described Zalando’s strategy (Plan B) using OAuth 2.0 between different AWS accounts. At Zalando, autonomous teams are important which is reflected in their architecture as separate AWS accounts for teams. During the talk, considerations such as token revocation and decentralization were covered.

![](img/0__yQk12cOVzm3ogK9m.jpg)

**Lightning Talk** — _Florian Leibert, Founder/CEO, Mesosphere_. The lightning talk by the founder of Mesosphere, Florian Leibert, started out with a rundown of Florian’s experiences with Mesos at Twitter and AirBnb which led to Mesosphere. An anecdote about Twitter’s previous scalability issues before Mesos was presented as the “Justin Bieber Problem”. Afterwards an introduction to DC/OS was given followed by a demonstration of a Twitter-like sample application called [Tweeter](https://github.com/mesosphere/tweeter). A similar demonstration of Mesos and DC/OS using Tweeter can be [found here](https://www.youtube.com/watch?v=JXrDQQe7Ud4). Two great articles about Mesos are, [Why the data center needs an operating system](https://people.csail.mit.edu/matei/papers/2011/hotcloud_datacenter_os.pdf) and [Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center](https://people.eecs.berkeley.edu/~alig/papers/mesos.pdf)

![](img/0__AxkQnohScIwmykRz.jpg)

**Efficient monitoring in modern environments** — _Tobias Schmidt, SoundCloud_ In this talk, Tobias presented his and SoundCloud’s best practices on monitoring from their experience. SoundCloud has developed and open sources their monitoring solution, [Prometheus](http://kubecloud.io/containerdays-hamburg/prometheus.io), which has become a [Cloud Native Computing Foundation](https://cncf.io/) project. The talk consisted of four parts: monitoring, metrics, four golden signals, and alerting. Several thoughtful considerations were presented such as the risk of alert fatigue if alerts are triggered to easily when they aren’t important. Among other examples were the importance of runbooks/playbooks which are a concise description of a potential solutions when an alert is triggered. Several of the ideas overlap with what is described in the book: [Site Reliability Engineering — How Google Runs Production Systems](http://shop.oreilly.com/product/0636920041528.do). Tobias’ [slides](https://speakerdeck.com/grobie/efficient-monitoring-in-modern-environments) contain more details e.g. on which metrics to collect.

We took a break during the last talks before the last keynote since we had a long way to drive to reach Aarhus before midnight. Unfortunately, the last keynote with Aaron Huslage from Docker was cancelled due to illness. All in all, it was a very interesting day with a lot of inputs from other’s experiences and considerations combined with interesting conversations with other sharing the same passion.

_Originally published at_ [_kubecloud.io_](http://kubecloud.io/containerdays-hamburg/) _on July 13, 2016._