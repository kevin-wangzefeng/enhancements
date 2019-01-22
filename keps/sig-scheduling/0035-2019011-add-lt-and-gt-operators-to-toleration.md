---
kep-number: 35
title: Add LT and GT operators to toleration
authors:
  - "@kevin-wangzefeng"
owning-sig: sig-scheduling
participating-sigs:
  - sig-scheduling
reviewers:
  - TBD
approvers:
  - TBD
editor: TBD
creation-date: 2019-01-11
last-updated: 2019-01-11
status: provisional
---

# Add LT and GT operators to toleration


## Table of Contents

A table of contents is helpful for quickly jumping to sections of a KEP and for highlighting any additional information provided beyond the standard KEP template.
[Tools for generating][] a table of contents from markdown are available.

* [Table of Contents](#table-of-contents)
* [Summary](#summary)
* [Motivation](#motivation)
    * [Goals](#goals)
    * [Non-Goals](#non-goals)
* [Proposal](#proposal)
    * [User Stories [optional]](#user-stories-optional)
      * [Story 1](#story-1)
      * [Story 2](#story-2)
    * [Implementation Details/Notes/Constraints [optional]](#implementation-detailsnotesconstraints-optional)
    * [Risks and Mitigations](#risks-and-mitigations)
* [Graduation Criteria](#graduation-criteria)
* [Implementation History](#implementation-history)
* [Drawbacks [optional]](#drawbacks-optional)
* [Alternatives [optional]](#alternatives-optional)

[Tools for generating]: https://github.com/ekalinin/github-markdown-toc

## Summary

TBD

```md
The `Summary` section is incredibly important for producing high quality user focused documentation such as release notes or a development road map.
It should be possible to collect this information before implementation begins in order to avoid requiring implementors to split their attention between writing release notes and implementing the feature itself.
KEP editors, SIG Docs, and SIG PM should help to ensure that the tone and content of the `Summary` section is useful for a wide audience.

A good summary is probably at least a paragraph in length.
```

## Motivation

Kubernetes default scheduler, by default, schedules pods “equally” distributed across all the nodes in a cluster. This, in turn, may result in resource fragmentation, and pods with large resource requirements may not get scheduled due to it.

It may be not a big problem in cases most workloads are long running, since:
- HA is most important (at least more important than resource utilization).
- Resource requirements don’t often change, and
- Cluster admins may work periodically on checking free resource and resource planning

While for batch oriented workloads, it becomes a real problem:
- Resource utilization is more important than HA (batch jobs may even not require HA)
- Hard to estimate resource requirement
- Having large-size pods pending costs
- User would sometimes rather have small-size pods pending than being evicted

For example, in Genome/DNA Sequencing cases, the sequencing workflows usually come with large-size pods in beginning steps, followed by a bunch of small size pods. Pending large-size pods will block corresponding workflows from the very beginning.

This particular proposal involves avoiding such resource fragmentation in a cluster using taint-toleration functionality **by reserving nodes for large pods**, specifically by adding “GT” and “LT” toleration.operator values.

### Goals

TBD

### Non-Goals

TBD

## Proposal

### Add LT and GT operators to toleration
As it’s hard to predict when a pod comes with large resource requests, the easiest way to implement is using resource reservation.

Original semantic would look like the following:

- Add some special labels on nodes, saying that:
> I’m reserved for only pods requesting more than 4U8G,  
> please don’t schedule pods requesting less than 4U8G on me.


Ideally, to implement this semantic, we may have something like “podselector” on node, matching pod requests.
And when scheduling pods, scheduler checks pod resource requests and the podselector on nodes, to prevent inappropriate pod from scheduling onto the reserved node.

In order to have minimal API changes, we can implement this with current taint-tolerations functionality. We just need to have toleration provide support for GT, LT operators.






```md
This is where we get down to the nitty gritty of what the proposal actually is.
```

### User Stories [optional]

Detail the things that people will be able to do if this KEP is implemented.
Include as much detail as possible so that people can understand the "how" of the system.
The goal here is to make this feel real for users without getting bogged down.

#### Story 1

#### Story 2


### API change

### Implementation Details/Notes/Constraints [optional]

What are the caveats to the implementation?
What are some important details that didn't come across above.
Go in to as much detail as necessary here.
This might be a good place to talk about core concepts and how they releate.

### Risks and Mitigations

What are the risks of this proposal and how do we mitigate.
Think broadly.
For example, consider both security and how this will impact the larger kubernetes ecosystem.

## Graduation Criteria

TBD

How will we know that this has succeeded?
Gathering user feedback is crucial for building high quality experiences and SIGs have the important responsibility of setting milestones for stability and completeness.
Hopefully the content previously contained in [umbrella issues][] will be tracked in the `Graduation Criteria` section.

[umbrella issues]: https://github.com/kubernetes/kubernetes/issues/42752

## Implementation History

TBD

Major milestones in the life cycle of a KEP should be tracked in `Implementation History`.
Major milestones might include

- the `Summary` and `Motivation` sections being merged signaling SIG acceptance
- the `Proposal` section being merged signaling agreement on a proposed design
- the date implementation started
- the first Kubernetes release where an initial version of the KEP was available
- the version of Kubernetes where the KEP graduated to general availability
- when the KEP was retired or superseded


## Possible solutions using existing K8s capabilities
### NodeAffinity
Label nodes with two labels types: min-mem={min-value}, max-mem={max-value}.
And every pod needs to come with nodeAffinity configuration e.g. min-mem<{request.mem} and max-mem>{request.mem}.
A dynamic admission controller may be used to auto inject proper nodeAffinity configuration to every pod.

Pros:
1. NodeAffinity supports hard (`requiredDuringScheduling`) and soft (`preferredDuringScheduling`) policy, user could either reserve some nodes for large-size pods with restriction or try to co-locate small-size pods with preference.
2. `NodeSelectorOperator` supports GT and LT operator, easy to represent with threshold semantic.
Cons:
1. Need to add labels to all nodes if user wants to achieve hard resource reservation.
2. Need to make sure all pods indicate nodeAffinity rules.
3. Not easy to apply on a live cluster with running pods.
4. Not quite flexible in the long run, e.g. after a period of time, user may want to change the reservation rule/threshold.

### InterPodAffinity
Co-locating pods to a set of nodes that has the same label key and value (indicated by topology-key).
All pods need to be labeled with resource requirement e.g. cpu=2.

Pros:
1. PodAffinity supports hard (`requiredDuringScheduling`) and soft (`preferredDuringScheduling`) policy, user could either co-locate small-size pods with restriction or with preference.
Cons:
1. `LabelSelectorOperator` doesn’t support GT and LT operator, not easy to select pods with threshold semantic.
2. As is an affinity to running pods, the pods may co-locate on a set of nodes SetA in the beginning and later on “moved to” another set of nodes SetB in the long run.
3. More complicated than NodeAffinity


### MostRequestedPriority
Prefer co-locate pods on to most requested nodes, the preferred nodes may change in the long run.
Not able to implement hard resource reservation.


### Pority/Preemption
Simply set large resource requesting pods with higher priority. Currently pod priority/preemption functionality sort of helps, but can’t deal with all the cases.

The key mechanism of pod priority is to try scheduling pods with higher priority every time prior to scheduling a pod. It may cooperate with preemption (e2e priority), or not (scheduling time priority).
However, even if large pods are given with higher priority, they may not get scheduled due to resources divided into pieces. In this case, small pods are always much easier to get scheduled even if they don’t have high priority.

Preemption (passive rescheduling) or Active Rescheduling (enforced by descheduler) needs to evict some running pods to make room for failed-scheduling/high priority/large resource requests pods (for batch oriented workloads). Evicting a running pod may cause part or the whole computing state loss, which is really waste of precious resource usage time. Even worse, in cases where some batch workloads don’t support rerun from middle state/checkpoint, they have to be rerun from the very beginning.


## Alternatives

### Fine-grained priority/preemption (Fairness preemption)
Current preemption only happens between different priority classes, the cons are:
1. Not easy for users to set up priority.
2. Resource fragmentation usually happens in cases when most pods are in the same priority class.
3. But different pods(jobs) actually do have different priorities.
4. If a pod with large resource request can’t get scheduled while competing with pods requesting small amount of resources, it will probably never get scheduled -- small pods always get scheduled, which is not fair. (Even if enabled priority for only during scheduling -- just try scheduling large pod every time prior to other pods).
5. But if we enable preemption and give large-size pods higher priority, it will always evict running pod. But user may not want running pods get evicted (concern of causing disruption for long running services, or, waste for jobs since they may have to reschedule/rerun).

The idea is to: balance between trade-off for evicting a running pod(job) and cost of keeping a pod(job) pending.
E.g.:
-  Fairness with preemption for queue based scheduler (could be applied to default scheduler or kube-batch)
   refer to [Quincy: Fair Scheduling for Distributed Computing Clusters][Quincy scheduler paper]
- Quincy cost model for Firmament/Poseidon.
   refer to [Firmament: Fast, Centralized Cluster Scheduling at Scale][Firmament scheduler paper]



## Infrastructure Needed [optional]

Use this section if you need things from the project/SIG.
Examples include a new subproject, repos requested, github details.



[Quincy scheduler paper]:https:://www.sigops.org/s/conferences/sosp/2009/papers/isard-sosp09.pdf
[Firmament scheduler paper]:https:://www.usenix.org/system/files/conference/osdi16/osdi16-gog.pdf

