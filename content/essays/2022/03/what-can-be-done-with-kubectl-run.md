---
title: "What Can Be Done With 'kubectl run' Command?"
date: 2022-03-18
draft: false
authors: Hugo Martins
summary: "'kubectl run' is a little command inside kubectl that can be used to run small experiments and create pods fairly quickly in Kubernetes."
authors: Hugo Martins
categories: ["kubernetes"]
popular: false
series: ["ckad"]
---

[kubectl run](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run) is a command that can *"create and run a particular image in a pod"*, which means that it will start a pod with a particular image. It can also create the pod with some particular details by using combinations of flags. We can see the code for it in [pkg/cmd/run/run.go](https://github.com/kubernetes/kubectl/blob/f48256c8eef5df2e3a9c621dd667839bdbe7c4cd/pkg/cmd/run/run.go).

## Interesting Flags

- `--image` specifies the image to be executed.
- `--command` allows to overwrite the command field in the container with whatever arguments are sent after `--`.
- `--env` to set environment variables in the pods, in the form of `key:value` and one variable per `--env`  flag.
- `--expose` and `--port` to create a ClusterIP associated with the pod.
- `--labels` which allows us to configure labels for the specific pod. Labels are accepted with only one flag, separated by a comma.
- `--dry-run` for testing purposes.

## Examples

From documentation:

```
kubectl run nginx --image=nginx
kubectl run hazelcast --image=hazelcast/hazelcast --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default"
kubectl run hazelcast --image=hazelcast/hazelcast --labels="app=hazelcast,env=prod"
kubectl run nginx --image=nginx --command -- <cmd> <arg1> ... <argN>
```

## Use Case

While I don't see much use in `kubectl run` for dealing with production grade systems, it can be a great resource for testing out different configurations of pods and creating a bunch of pods if there's a need for it. It can also be a great introductory command for when first starting to deal with Kubernetes because it eases the interaction with Kubernetes without having to deal with YAML right away.

Due to its `--output` flag, it can also be a great way to use `kubectl` to create pods, in a stable way, from scripts because we can have stable and reproducible outputs from it. The fact that we can use a `--dry-run` flag is also a big plus.
