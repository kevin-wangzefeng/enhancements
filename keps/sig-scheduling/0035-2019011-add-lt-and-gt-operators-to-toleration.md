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

The `Summary` section is incredibly important for producing high quality user focused documentation such as release notes or a development road map.
It should be possible to collect this information before implementation begins in order to avoid requiring implementors to split their attention between writing release notes and implementing the feature itself.
KEP editors, SIG Docs, and SIG PM should help to ensure that the tone and content of the `Summary` section is useful for a wide audience.

A good summary is probably at least a paragraph in length.

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

This particular proposal involves avoiding such resource fragmentation in a cluster using taint-toleration functionality **by reserving nodes for large pods**, specifically by adding “Gt” and “Lt” toleration.operator values.

### Goals

TBD

### Non-Goals

TBD

## Proposal

This is where we get down to the nitty gritty of what the proposal actually is.

### User Stories [optional]

Detail the things that people will be able to do if this KEP is implemented.
Include as much detail as possible so that people can understand the "how" of the system.
The goal here is to make this feel real for users without getting bogged down.

#### Story 1

#### Story 2

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


## Alternatives [optional]

Similar to the `Drawbacks` section the `Alternatives` section is used to highlight and record other possible approaches to delivering the value proposed by a KEP.

## Infrastructure Needed [optional]

Use this section if you need things from the project/SIG.
Examples include a new subproject, repos requested, github details.
L
