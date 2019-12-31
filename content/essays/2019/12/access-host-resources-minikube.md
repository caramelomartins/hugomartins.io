---
title: "How to Access Host Resources in Minikube Pods?"
date: 2019-12-31
categories:
  - minikube
---

This title is self-explanatory but, in any case, how can I access host resources in `minikube`? It seems straightforward but after digging up through the internet, it seems that it isn't as easy as it ought to be. <!--more-->

I tried answering this question this week and it took me far longer than I was expecting. There are two directions of communication between pods in a `minikube` cluster and their host machine: from the pods to the host machine and from the host machine to a specific pod. From host to pod seems to be easier, what with all the endless Kubernetes tutorials on how to expose ports in pods. Communicating from within a pod with the host machine seems to be more of a mess.

I found a [series](https://stackoverflow.com/questions/55164223/access-mysql-running-on-localhost-from-minikube) [of](https://stackoverflow.com/questions/43354167/minikube-expose-mysql-running-on-localhost-as-service?noredirect=1&lq=1<Paste>) [answers](https://stackoverflow.com/questions/49289009/during-local-development-with-kubernetes-minikube-how-should-i-connect-to-postg?noredirect=1&lq=1) with a quick search. Some were helpful, while a lot of them spoke about how to connect from host machine to a pod inside a `minikube` cluster, rather than the opposite. Nonetheless, amongst those answers I found a Github [issue](https://github.com/kubernetes/minikube/issues/2735).

In the issue, the fact that Docker for Mac has this [feature](https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds) is discussed, in which one can use a crafted DNS name - `host.docker.internal` - to communicate from a pod with resources running on the host machine. Nothing of the sort seemed to exist for `minikube` for a while. A discussion ensued and, eventually, it was documented in `minikube`'s [documentation](https://minikube.sigs.k8s.io/docs/tasks/accessing-host-resources/).

In those documents, we can see that, to know what IP the host machine has inside the `minikube` cluster, we can execute the following command:

```
$ minikube ssh "route -n | grep ^0.0.0.0 | awk '{ print \$2 }'"
```

Currently, there are a few caveats:

> The service running on your host must either be bound to all IP’s (0.0.0.0) and interfaces, or to the IP and interface your VM is bridged against. If the service is bound only to localhost (127.0.0.1), this will not work.

Could be worse! At least, there's a known way of communicating with the host machine, from the pods inside the `minikube` cluster. Now, let's try to understand what that command means:

- `minikube ssh` means that the following command, or sequence of commands, is executed inside the VM hosting the `minikube` cluster.
- `route -n` lists all the kernel routing tables.
- `grep ^0.0.0.0` selects only the routing tables that have as a destination `0.0.0.0`, or more specifically all IP addresses on all interfaces of this machine.
- `awk '{print $2}` will print the second element of the string piped from `grep`, which corresponds to the gateway for that route.

Apparently, in the case of `minikube`, the bridge IP in the routing tables is the IP address that you can use to communicate with your host machine. The reason for this seems to be something called bridge networking, where a virtual ethernet adapter from a virtual machine connects to a network through the host machine's ethernet adapter - you can read more about this on [VMWare](https://www.vmware.com/support/ws4/doc/network_bridged_ws.html), for example.

If you care to go deeper into the code, some of this can also be gotten from there. Looking at [pkg/minikube/cluster/cluster.go](https://github.com/kubernetes/minikube/blob/master/pkg/minikube/cluster/cluster.go), we can see each VM driver has a particular mapping of VM host IP, in function `GetVMHostIP`. For example, HyperKit maps to `192.168.64.1` statically, while VMWare has something that _appears_ more dynamic.

Nonetheless, now you have the IP with which you can communicate from inside a pod with the host machine. You need to choose how to use it. You can bind it as an environment variable, hardcode it into manifests or use a separate resource for it. In the end, it will always depend on your use case.
