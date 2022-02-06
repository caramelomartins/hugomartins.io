---
title: "KubeCon Europe 2021: Highlights  #1"
date: 2021-05-04T15:04:59+01:00
draft: false
authors: Hugo Martins
categories: [kubecon, kubernetes]
summary: "Stream of consciousness notes about my experience during KubeCon Europe 2021 and cool things I've learned or enjoyed."
series: ["kubecon2021"]
---

![kubecon-logo](https://raw.githubusercontent.com/cncf/artwork/master/other/kubecon-cloudnativecon/2021-eu-virtual/color/kubecon-eu-2021-color.png)

These will be a series of notes written as a _stream of consciousness_ without much editing. This year's edition of KubeCon Europe was once again completely virtual. Although there was some confusion with understanding the platform, setting up a calendar and finding where all the sessions were occurring (e.g. co-located events run on their platforms), this first day was interesting and instructive.

## Sessions

<blockquote align="center" class="twitter-tweet"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/KubeCon?src=hash&amp;ref_src=twsrc%5Etfw">#KubeCon</a> setup. Starting off with LitmusChaos office hours. <a href="https://t.co/c7Q24gl6Dd">pic.twitter.com/c7Q24gl6Dd</a></p>&mdash; Hugo Martins (@caramelomartins) <a href="https://twitter.com/caramelomartins/status/1389510179021017089?ref_src=twsrc%5Etfw">May 4, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

I started my day off with "Project Office Hours" by [LitmusChaos](https://litmuschaos.io/) maintainers. I have watched LitmusChaos' presentation in FOSDEM 2021 and I am amazed by the progress of the project so far, in a matter of months. LitmusChaos is CNCF sandbox project which aims to be a cloud native approach to [Chaos Enginering](https://en.wikipedia.org/wiki/Chaos_engineering) in Kubernetes. It allows users to create experiments in a declarative approach, providing extensibility SDKs, and workflows that are easy to build quite quickly ("start in minutes not in days"). I believe LitmusChaos can pave the way for accessible and agile Chaos Engineering and I'm excited to be able to try some experiments and give it a go.

<blockquote align="center" class="twitter-tweet"><p lang="en" dir="ltr">Great session at <a href="https://twitter.com/hashtag/KubeCon?src=hash&amp;ref_src=twsrc%5Etfw">#KubeCon</a> by <a href="https://twitter.com/ronald_petty?ref_src=twsrc%5Etfw">@ronald_petty</a>! &quot;First Principles in Cloud Native Technology&quot; was both instructive and entertaining. It helps you understand how to break down all the complexity of Kubernetes and its ecosystem. I&#39;d recommend it to every one.</p>&mdash; Hugo Martins (@caramelomartins) <a href="https://twitter.com/caramelomartins/status/1389551903743352836?ref_src=twsrc%5Etfw">May 4, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Next, I watched Ronald Petty's _"First Principles of Cloud Native Technology"_ about how to drill down into all of the technologies in the Kubernetes and Cloud ecosystem. Ronald Petty made a case about going back to first principles, which essentially means breaking down all of the complexity we are currently looking at into small problems until we can understand each of the small concepts and then compound that knowledge from the ground up. A simple example is breaking down "Cloud", into "Kubernetes", into "Pods", into "Containers" and then "Process". With this break down we can start from the "first principles" and understand each concept and associated technology until we have a complete picture of the macro systems in our mind and we can absorb that knowledge more easily. I believe this strategy applies to almost everything and reminds me of the [The Feynman Technique](https://fs.blog/2021/02/feynman-learning-technique/) as a way to advance our knowledge and learn new things. Slides are available [here](https://static.sched.com/hosted_files/kccnceu2021/58/FirstPrinciplesOfCloudNativeTechnologyKubeCon_EU_2021.pdf).

<blockquote align="center" class="twitter-tweet"><p lang="en" dir="ltr">Watching <a href="https://twitter.com/k_gamanji?ref_src=twsrc%5Etfw">@k_gamanji</a>&#39;s session @ <a href="https://twitter.com/hashtag/KubeCon?src=hash&amp;ref_src=twsrc%5Etfw">#KubeCon</a> about open standards renews my faith in open technology. Only open technology can improve innovation, enhance extensibility and interoperability. It also helps improve technology&#39;s resilience and stability, as well as share knowledge!</p>&mdash; Hugo Martins (@caramelomartins) <a href="https://twitter.com/caramelomartins/status/1389554038849556480?ref_src=twsrc%5Etfw">May 4, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

Another surprising session for me was Katie Gamanji's _"Open Standards: Anchoring Extensibility for Cloud-Native"_ where Katie Gamanji laid down the foundations of how open standards have been a core foundation of expanding Kubernetes, and its ecosystem, and improving interoperability. Container Runtime Interface (CRI), Container Network Interface (CNI), Container Storage Interface (CSI), Service Mesh Interface (SMI), Cloud Provider Interface (CPI). All of these interfaces, and associated projects, are based on open standards. Only open technology can improve innovation, enhance extensibility and interoperability. It also helps improve technology's resilience and stability, as well as share knowledge and experience across the entire industry. As I said in the above tweet, this session renewed my faith in open technology and open standards! I'm not sure if this session will be available but if it is, I'd highly recommend people to watch it.

I also watched _"Charting our K8s and Cloud Native Journey: Past, Present and Future"_ where some folks described and discussed Datadog's experience with Kubernetes. I believe the most interesting stake about this roundtable has been how Kubernetes has evolved from an experimental technology to "boring technology" that most companies would be willing to depend on and run in production systems. It has been less than a decade but the ecosystem has been evolved at such a fast pace that it has started to bring some stability and maturity that enables it to be a runtime dependency for different types of workloads at truly enormous scales. At the same time, while providing clear business values, Kubernetes has also evolved operationally which has made running Kubernetes much smoother. The future seems bright and exciting.

Lastly, I enjoyed _"Vitess, are we Fast Yet?"_ by Akilan Selvacoumar and Florent Poinsard from Planetscale to get a quick glimpse at what Planetscale is doing to benchmark Vitess and ensure its performance is improving. They are using [arewefastyet](https://github.com/vitessio/arewefastyet/) which is a tool purposefully built to perform benchmarks on every push, merge and release that happens in Vitess. This is something I've been dreaming about having but never actually saw it pulled off nicely like this.

## Cool Tech

A list of technologies that caught my attention and that I find interesting:

- [LitmusChaos](https://litmuschaos.io/): "Chaos Engineering for your Kubernetes".
- [Longhorn](https://longhorn.io/): "Cloud native distributed block storage for Kubernetes".
- [kured](https://github.com/weaveworks/kured): "Kubernetes Reboot Daemon".
- [renovate](https://github.com/renovatebot/renovate): "Universal dependency update tool that fits into your workflows".
- [Falco](https://falco.org/): "Cloud-Native runtime security".
- [cri-o](https://cri-o.io/): "Lightweight Container Runtime for Kubernetes".
- [gVisor](https://gvisor.dev/): "an application kernel for containers that provides efficient defense-in-depth anywhere".
- [rook](https://rook.io/): "Production ready management for File, Block and Object Storage".

I haven't explored much of the sponsored demos or the exhibition demos though, these are all from the sessions I attended.