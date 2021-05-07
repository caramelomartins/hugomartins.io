---
title: "KubeCon Europe 2021: Highlights  #4"
date: 2021-05-07T10:27:59+01:00
draft: false
authors: Hugo Martins
categories: [kubecon, kubernetes]
summary: "Stream of consciousness notes about my experience during KubeCon Europe 2021 and cool things I've learned or enjoyed."
---

![kubecon-logo](https://raw.githubusercontent.com/cncf/artwork/master/other/kubecon-cloudnativecon/2021-eu-virtual/color/kubecon-eu-2021-color.png)

Today was the last day of KubeCon Europe 2021. As I have done in the last three days, it is only right that I write about what today was like for me. You can find my highlights for previous days in [KubeCon Europe 2021: Highlights #1]({{< ref "essays/2021/05/kubecon-europe-2021-highlights-1"  >}}), [KubeCon Europe 2021: Highlights #2]({{< ref "essays/2021/05/kubecon-europe-2021-highlights-2"  >}}) and [KubeCon Europe 2021: Highlights #3]({{< ref "essays/2021/05/kubecon-europe-2021-highlights-3"  >}}). These are a series of notes written as a _stream of consciousness_ without much editing. This year's edition of KubeCon Europe was once again completely virtual.

## Sessions

**When Prometheus Can’t Take the Load Anymore** by Liron Cohen was the first good session I watched today. This session focused on showcasing some issues with Prometheus such as scalability issues, no real high availability, no global view of information and no long term storage. Then Liron Cohen explained, in quite some detail, three potential solutions: [M3](https://eng.uber.com/m3/), [Cortex](https://cortexmetrics.io/) and [Thanos](https://thanos.io/). In the end, they used Thanos because of the simplicity of its architecture when compared with M3 and Cortex. I had no idea about the complexity that is needed to have a highly available Prometheus which is why this session was so enlightening for me.

On a smaller scale, **SIG CLI: Intro and Updates** by Maciej Szulik, Katrina Verey and Jeff Regan, was also interesting because I wanted to know more about this SIG. Updates with better integration between kubectl and kustomize were presented, as well as some other tooling that I was unaware of such as [kui](https://github.com/kubernetes-sigs/kui) and [krew](https://krew.sigs.k8s.io/). A funny thing I discovered was that they used a pattern in kubectl development, which I have seen before, of coupling commands (from Cobra) with business logic. They are now trying to go back on that decision, which is something I've also faced before because coupling the framework with the business logic ends up creating a lot of interoperability issues in the business logic.

Next up is **Battle of Black Friday: Proactively Autoscaling StockX** by Mario Loria and Kyle Schrade. This session started as a story of going from manual scaling to proactive automated scaling. It was a wonderful session about proactive scaling, going further than Cluster Autoscaling and HPA. They've developed a system that can easily handle spikes in traffic of more than 500% in less than 30 seconds by having an endpoint consuming events from Kubernetes and then, based on those events, having a CronJob warming up and cooling down more resources, proactively, A great example of how to deal with the delays of cluster autoscaling and HPA when your spikes are not gradual, non-linear and unannounced. Also a great example of keeping systems simple, with a single endpoint and a CronJob to manage the warming up and cooling down proactively.

**How We are Dealing with Metrics at Scale on GitLab.com** by Andrew Newdigate. Showcased a solution for mitigating low precision, unactionable and flappy alerting. It leverages key metrics, such as Apdex, Requests per Second, Errors per Second and Saturation, as SLIs to then be able to monitor if SLOs are being met. Metrics are centralized and shared in a metrics catalogue that defines key metrics, per-service SLOs, inter-service dependencies, and generates Prometheus and Thanos rules, as well as, Grafana dashboards. Using Grafana as a launchpad to other components can also help with context during on call.

One great discovery for me today was [Gateway API](https://gateway-api.sigs.k8s.io/) presented in **Gateway API: A New Set of Kubernetes APIs for Advanced Traffic Routing** by Harry Bagdi and Rob Scott. It provides a set of resources in Kubernetes that improve on what already exists with Ingresses. It focuses on improved routing and can handle much more complex scenarios, is more expressiveness and extensive. It can handle canary deployments (traffic splitting) much more easily, header matching and multicluster traffic. It also supports HTTP, UDP and TCP routing through simple and expressiveness resources. I believe this is a truly ground-breaking thing in this niche.

**Cert-Manager Beyond Ingress – Exploring the Variety of Use Cases** by Matthew Bates was a nice introduction to a lot of use cases of cert-manager that I was unaware. Apparently, you can use cert-manager in almost all layers of Kubernetes. Pod to Pod, Kubelet, kubeadm and webhooks. I had no idea cert-manager had all of this reach.

**High Throughput with Low Resource Usage: A Logging Journey** by Eduardo Silva featured a deep dive into the complexities of log collectors and processors such as Fluentd. It also went into some detail on how [Fluent Bit](https://fluentbit.io/) can optimize data handling and I/O with data serialization and buffer management via chunks. There are a couple of slides that are particularly enlightening in terms of the complexity that a log collect and processor has to handle.

I still had a few more sessions on "to watch" such as **K8s Labels Everywhere! Decluttering With Node Profile Discovery.** by Conor Nolan, **Isolate the Users! Supporting User Namespaces in K8s for Increased Security** by Mauricio Vásquez, **Application Autoscaling Made Easy With Kubernetes Event-Driven Autoscaling** by Tom Kerkhove, and **Choose Wisely: Understanding Kubernetes Selectors** by Christopher Hanson. Given that the sessions are available on-demand on KubeCon's platform I will potentially watch them later next week. There was just so much to see that you can't get everywhere, otherwise, you'd be awake for 4 days straight.

## Cool Tech

- [ketpn](https://keptn.sh/): "Cloud-native application life-cycle orchestration".
- [M3](https://eng.uber.com/m3/): "Uber’s Open Source, Large-scale Metrics Platform for Prometheus"
- [Cortex](https://cortexmetrics.io/): "Horizontally scalable, highly available, multi-tenant, long term Prometheus".
- [Thanos](https://thanos.io/): "Open source, highly available Prometheus setup with long term storage capabilities".
- [kui](https://github.com/kubernetes-sigs/kui): "hybrid command-line/UI development experience for cloud-native development".
- [krew](https://krew.sigs.k8s.io/): " plugin manager for kubectl command-line tool".
- [Virtual Clusters](https://github.com/kubernetes-sigs/multi-tenancy/tree/master/incubator/virtualcluster): "a new architecture to address various Kubernetes control plane isolation challenges".
- [Goldilocks](https://github.com/FairwindsOps/goldilocks): "utility that can help you identify a starting point for resource requests and limits".
- [Rook](https://rook.io/): "Production ready management for File, Block and Object Storage".
- [Ceph](https://docs.ceph.com/en/latest/): "uniquely delivers object, block, and file storage in one unified system".
- [Gateway API](https://gateway-api.sigs.k8s.io/): "a collection of resources that model service networking in Kubernetes".
- [Fluent Bit](https://fluentbit.io/)