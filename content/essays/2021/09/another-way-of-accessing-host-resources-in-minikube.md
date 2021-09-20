---
title: "Another Way of Accessing Host Resources in Minikube"
date: 2021-09-20
draft: false
authors: Hugo Martins
categories: [minikube]
summary: "An update on a previous essay I wrote about accessing host resources on Minikube which has now become outdated. Minikube now has improved on this and has a much better approach."
---

A few years ago, I wrote an essay about this topic titled "[How to Access Host Resources in Minikube Pods?]({{< ref "essays/2019/12/access-host-resources-minikube.md" >}})". In it, I have described how to access host resources in Minikube. Something I thought should be easy but, at the time, it really wasn't.

Back then, I've described an approach, based on a Github [issue](https://github.com/kubernetes/minikube/issues/2735), relying on listing kernel routing tables, that allowed us to fetch the IP of the host machine inside Minikube. I described why this worked, going so far as sharing a few snippets of code from the Kubernetes source code.

While this has served me well for the last few years, I've recently discovered that this approach is a bit outdated - the documentation on that essay doesn't even exist anymore. I wrote this when Minikube was v1.6.2 and it is now v1.23.1...a lot has happened.  After searching a bit more, I've understood that Minikube _now_ has a proper solution for this, documented in [Host access](https://minikube.sigs.k8s.io/docs/handbook/host-access/).

This approach applies for Minikube bigger than v1.10 and it essentially relies on using `host.minikube.internal` in order to connect to the host Minikube is running on. According to the documentation, "minikube v1.10 adds a hostname entry host.minikube.internal to /etc/hosts." I've tested this and it seems to actually work, while being much easier and reliable.

As with the initial approach, the same caveat applies:

> The service running on your host must either be bound to all IP’s (0.0.0.0) and interfaces, or to the IP and interface your VM is bridged against. If the service is bound only to localhost (127.0.0.1), this will not work.