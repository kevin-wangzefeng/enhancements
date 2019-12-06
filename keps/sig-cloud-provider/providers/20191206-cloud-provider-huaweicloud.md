---
kep-number: 
title: Cloud Provider For HUAWEI CLOUD
authors:
  - "@RainbowMango"
owning-sig: sig-cloud-provider
reviewers:
  - "@andrewsykim"
  - "@cheftako"
approvers:
  - "@andrewsykim"
  - "@cheftako"
editor: TBD
creation-date: 2019-12-06
last-updated: 2019-12-06
status: provisional
see-also:
replaces:
superseded-by:
---

# Cloud Provider For HUAWEI CLOUD

This is a KEP for adding `Cloud Provider For HUAWEI CLOUD` into the Kubernetes ecosystem.

## Table of Contents

<!-- toc -->
- [Summary](#summary)
- [Motivation](#motivation)
  - [Goals](#goals)
  - [Non-Goals](#non-goals)
- [Prerequisites](#prerequisites)
  - [Repository Requirements](#repository-requirements)
  - [User Experience Reports](#user-experience-reports)
  - [Testgrid Integration](#testgrid-integration)
  - [CNCF Certified Kubernetes](#cncf-certified-kubernetes)
  - [Documentation](#documentation)
  - [Technical Leads are members of the Kubernetes Organization](#technical-leads-are-members-of-the-kubernetes-organization)
- [Proposal](#proposal)
  - [Subproject Leads](#subproject-leads)
  - [Repositories](#repositories)
  - [Meetings](#meetings)
  - [Others](#others)
<!-- /toc -->

## Summary

[HUAWEI CLOUD Controller Manager](https://github.com/huawei-cloudnative/cloud-provider-huaweicloud) is an external cloud 
controller manager for running kubernetes in a HUAWEI Cloud cluster.

## Motivation

### Goals

`Cloud Provider For HUAWEI CLOUD` provides an external cloud controller manager for users.

### Non-Goals

## Prerequisites

### Repository Requirements

[HUAWEI CLOUD Controller Manager](https://github.com/huawei-cloudnative/cloud-provider-huaweicloud) is a implementation of
[Kubernetes Cloud Controller Manager](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/). 

### User Experience Reports

There must be a reasonable amount of user feedback about running Kubernetes for this cloud provider. 
You may want to link to sources that indicate this such as github issues, product data, customer tesitimonials, etc.
TODO(RainbowMango): Need user feedback about running Kubernetes for this cloud provider.

### Testgrid Integration

Your cloud provider is reporting conformance test results to TestGrid as per the [Reporting Conformance Test Results to Testgrid KEP](https://github.com/kubernetes/community/blob/master/keps/sig-cloud-provider/0018-testgrid-conformance-e2e.md).
TODO(RainbowMango): 

### CNCF Certified Kubernetes

Your cloud provider is accepted as part of the [Certified Kubernetes Conformance Program](https://github.com/cncf/k8s-conformance).
TODO(RainbowMango): 

### Documentation

<--
There is documentation on running Kubernetes on your cloud provider as per the 
[cloud provider documentation KEP](https://github.com/kubernetes/community/blob/master/keps/sig-cloud-provider/0019-cloud-provider-documentation.md).
-->
TODO(RainbowMango): 

### Technical Leads are members of the Kubernetes Organization

<!--
All proposed technical leads for this provider must be members of the Kubernetes organization. Membership is used as a 
signal for technical ability, commitment to the project, and compliance to the 
[CNCF Code of Conduct](https://github.com/cncf/foundation/blob/master/code-of-conduct.md) which we believe are important 
traits for subproject technical leads. Learn more about Kubernetes community membership 
[here](https://github.com/kubernetes/community/blob/master/community-membership.md).
-->
Technical leads take the responsibility of maintain this project:
- @kevin-wangzefeng 
- @RainbowMango

## Proposal

<!--
This is where you can talk about what resources from the Kubernetes community you would like such as a repository in the 
Kubernetes organization to host your provider code.
-->
We need a repository under the Kubernetes organization to host our cloud provider specific implementation.
We'd like HUAWEI CLOUD provider would be a subproject of Kubernetes community. 

### Subproject Leads

<!--
This is where you indicate the leads for the subproject. Make sure you include their github handles. 
See the [SIG Charter](https://github.com/kubernetes/community/blob/master/sig-cloud-provider/CHARTER.md#subprojectprovider-owners) 
for more details on expectations from subproject leads.
-->
- @kevin-wangzefeng 
- @RainbowMango

### Repositories

<!--
This is where you propose a repository within the Kubernetes org, it's important you specify the name of the repository you would like. 
Cloud providers typically have at least 1 repository named `kubernetes/cloud-provider-foobar`. 
It's also important to indiciate who the initial owners of the repositories will be. 
These owners will be added to the initial OWNERS file. The owners of the subproject must be owners of the repositories 
but you can add more owners in the repo if you'd like. If you are requesting any repositories, be sure to add them to 
the SIG Cloud Provider [subproject list](https://github.com/kubernetes/community/tree/master/sig-cloud-provider#subprojects).
-->
We would like a repository named `kubernetes/cloud-provider-huaweicloud` to host HUAWEI CLOUD specific code.

The owner of the subproject can be the subject leads listed on above.

### Meetings

<!--
This where you specify when you will have meetings to discuss development of your cloud provider. 
SIG Cloud Provider will provide zoom/youtube channels as required. 
Note that these meetings are in addition to the biweekly SIG Cloud Provider meetings that 
subproject leads are strongly encouraged to attend.
-->
Recommended Meeting Time: Wednesdays at 20:00 PT (Pacific Time) (biweekly). [Convert to your timezone](http://www.thetimezoneconverter.com/?t=20:00&tz=PT%20%28Pacific%20Time%29).
- Meeting notes and Agenda.(TBD)
- Meeting recordings.(TBD)

### Others

Feel free to add anything else you may need.
