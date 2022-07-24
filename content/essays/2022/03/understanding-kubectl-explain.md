---
title: "Understanding 'kubectl explain'"
date: 2022-03-29
draft: false
authors: Hugo Martins
summary: "'kubectl explain' is a powerful tool to inspect details about resources, fields and their meanings. By using it, we can quickly see details of these things without having to open up a browser and associated documentation."
authors: Hugo Martins
categories: ["kubernetes"]
popular: false
series: ["ckad"]
---

[kubectl explain](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#explain) can be used to get more information about which fields a given resource needs, as well as the meaning behind those fields. It can be used in mainly two ways: either for getting information about the fields of a resource (e.g. `kubectl explain <resource>`) or by getting information about a specific field of a resource (e.g. `kubectl explain <resource>.<field/s>`).

Together with `api-versions` and `api-resources` it can quickly provide a lot of insight into Kubernetes and what needs to be written on a manifest to properly create or edit objects.

## Examples

Getting information about root fields of Pods:

```
$ kubectl explain pods
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion   <string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

   kind <string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

   metadata     <Object>
     Standard object's metadata. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

   spec <Object>
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

   status       <Object>
     Most recently observed status of the pod. This data may not be up to date.
     Populated by the system. Read-only. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

```

Getting information about the `spec` of Pods:

```
kubectl explain pods.spec
KIND:     Pod
VERSION:  v1

RESOURCE: spec <Object>

DESCRIPTION:
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

     PodSpec is a description of a pod.

FIELDS:
   activeDeadlineSeconds        <integer>
     Optional duration in seconds the pod may be active on the node relative to
     StartTime before the system will actively try to mark it failed and kill
     associated containers. Value must be a positive integer.

   affinity     <Object>
     If specified, the pod's scheduling constraints

   automountServiceAccountToken <boolean>
     AutomountServiceAccountToken indicates whether a service account token
     should be automatically mounted.

   containers   <[]Object> -required-
     List of containers belonging to the pod. Containers cannot currently be
     added or removed. There must be at least one container in a Pod. Cannot be
     updated.

   dnsConfig    <Object>
     Specifies the DNS parameters of a pod. Parameters specified here will be
     merged to the generated DNS configuration based on DNSPolicy.

   dnsPolicy    <string>
     Set DNS policy for the pod. Defaults to "ClusterFirst". Valid values are
     'ClusterFirstWithHostNet', 'ClusterFirst', 'Default' or 'None'. DNS
     parameters given in DNSConfig will be merged with the policy selected with
     DNSPolicy. To have DNS options set along with hostNetwork, you have to
     specify DNS policy explicitly to 'ClusterFirstWithHostNet'.

(...)

```

It explains what this field represents, as well as all the fields that can be configured inside this specific field, and their meanings. It can be a powerful tool to understand Kubernetes. A sort of  `man` for Kubernetes itself.

It can be particularly interesting to use `kubectl explain` to inspect, when you forget, [essential fields in Kubernetes manifests](https://hugomartins.io/essays/2022/02/essential-fields-in-kubernetes-manifests/), or internal details.
