---
title: "Essential Fields in Kubernetes Manifests"
date: 2022-02-15
draft: false
summary: "Kubernetes manifests are the baseline of describing objects in Kubernetes, their desired state and associated metadata. In this essay, we go over the four essential fields that every single Kubernetes resource needs."
authors: Hugo Martins
categories: ["kubernetes"]
popular: false
series: ["ckad"]
---

I recently started a quest to [complete CKAD](https://hugomartins.io/essays/2022/02/ckad-in-2022/) in the next few months, by May 2022. As I've explained in that previous essay, "I have spent quite some €€€ enrolling in it and I feel that it can still teach me a lot of relevant concepts about Kubernetes that will be useful" on my day-to-day.

While studying for CKAD, through [Kubernetes Certified Application Developer (CKAD) with Tests](https://www.udemy.com/course/certified-kubernetes-application-developer/), I've come to realize the importance of understanding the syntax of manifests in Kubernetes. Subconsciously, I obviously already knew this - the same way I know how important it is to dominate the syntax of a given programming language - but it is too easy to fall into a pattern of copy-pasting-and-changing, or simply filling in the gaps in already existing manifests.

Manifests in Kubernetes are the baseline of describing and defining resources, that we can then create and edit afterwards. Manifests represent the object specification describing "its desired state, as well as some basic information about the object (such as a name)." [^1] These manifests are most often described in `.yaml` files.

In essence, there are four essential fields in Kubernetes manifests that must be present in all manifests. These are: `apiVersion`, `kind`, `metadata` and `spec`. Each of these might have widely varying values populating them.

As an example, a starting point for a Kubernetes manifest would be:

```
apiVersion:
kind:
metadata:
spec:
```

## apiVersion

`apiVersion` allows us to define what version of the API a given resource is going to be using. It can be simply `v1`, which means it will be part of the _core_ API specified at `/api/v1`. It can also be `<name>/<version>`, for example `batch/v1`, specifying that at resource uses an API that is under `/api/<name>`. [^2]

We can find more about what APIs and versions exist on a given cluster by executing `kubectl api-version`:

```
$ kubectl api-versions
admissionregistration.k8s.io/v1
admissionregistration.k8s.io/v1beta1
apiextensions.k8s.io/v1
apiextensions.k8s.io/v1beta1
apiregistration.k8s.io/v1
apiregistration.k8s.io/v1beta1
apps/v1
authentication.k8s.io/v1
authentication.k8s.io/v1beta1
(...)

```

Results will differ from cluster to cluster, and between Kubernetes versions. We can have custom APIs, disabled APIs, or recent APIs could've been implemented in different Kubernetes versions.

`ReplicationController` and `ReplicaSet` are two popular objects that clarify this and differ in the `apiVersion`. `ReplicaController` is a component of the _core_ API in `v1` so we would write `apiVersion: v1` in its spec. `ReplicaSet`, which evolved from `ReplicaController`, is a component of a more recent API served at `apps/v1`. This sort of versioning is incredibly powerful and flexible, allowing Kubernetes to evolve while keeping a lot of backwards compatibility.

## kind

`kind` represents the kind of object that is specified via a manifest. Each kind of resource will be available on a particular API. This makes it essential that the specified `kind` and `apiVersion`  match on a specific manifest.

We can inspect which `kind` we can use for objects by executing `kubectl api-resources`:

```
kubectl api-resources
NAME                              SHORTNAMES   APIVERSION                        NAMESPACED   KIND
bindings                                       v1                                true         Binding
componentstatuses                 cs           v1                                false        ComponentStatus
configmaps                        cm           v1                                true         ConfigMap
apiservices                                    apiregistration.k8s.io/v1         false        APIService
controllerrevisions                            apps/v1                           true         ControllerRevision
daemonsets                        ds           apps/v1                           true         DaemonSet
deployments                       deploy       apps/v1                           true         Deployment
replicasets                       rs           apps/v1                           true         ReplicaSet
statefulsets                      sts          apps/v1                           true         StatefulSet

(...)

```

With `kubectl api-resources` we can quickly see as well which `apiVersion` needs to be specified in order to specify a particular resource.

## metadata

`metadata`  describes information of an object that allows for the unique identification of that object. When creating a manifest, this field should have _at least_ an associated name. Usually we will also see a field named `labels`.

## spec

`spec` defines the desired state of the object in Kubernetes. It will vary widely between different resources and API versions, which means that it can be tricky to figure out - or memorize - all the needed fields.

I've found that there's one instance of a resource that can be created without a `spec` field which is namespaces. If we create a namespace with only `apiVersion`, `kind` and `metadata`, creating the namespace with `kubectl create`, Kubernetes will accept that manifest _but_ it will create the namespace internally with an appropriate `spec`. As an example:

```
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

Running `kubectl create` results in:

```
$ kubectl create -f my-namespace.yaml
namespace/my-namespace created
```

Executing `kubectl get` will allows us to see the injected `spec` field:

```
$ kubectl ns my-namespace -o yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
  (...)
  selfLink: /api/v1/namespaces/test
  uid: f1b901a6-31d6-457a-aaaf-0cb6d600d52c
spec:
  finalizers:
  - kubernetes
status:
  phase: Active

```

---

All of this information can be confirmed in Kubernetes' own documentation by reading [Required Fields](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/#required-fields). Although this isn't a deep exploration of manifests, having solid bases can be extremely important to understand what has been built on top of this. Reasoning about this structure also provides a glimpse at the baseline that provides so much flexibility to Kubernetes, allowing it to have 50+ components _out-of-the-box_ and a lot of extensibility via custom resources.

[^1]: [Describing a Kubernetes object](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/#describing-a-kubernetes-object)
[^2]: [API Overview](https://kubernetes.io/docs/reference/using-api/)
